---
layout: post
title: "Using Benchee for Elixir Performance Tests"
date: 2019-07-20 18:15:05 -0700
categories: technical
cover: "/assets/Benchee.jpeg"
---
# Benchmarking Tests in Elixir

![Benchee](/assets/Benchee.jpeg)

Benchee is a useful tool if you find yourself in a situation where you need to test the limits of a system or need to define the behavior in specific environments. It seems primarily designed for development or debugging, but it can also be used in tests as well. The [github page](https://github.com/bencheeorg/benchee) is a useful tool for information about setup and configuration. There is more information on the [elixir school](https://elixirschool.com/en/lessons/libraries/benchee/) site as well.

These docs cover how to use Benchee. This post focuses on when to use it and other notes from my experience implementing a benchmarking test suite.  Benchee is useful for testing the limitations of the system, defining time constrains or memory impacts of critical processes, or as an aid in development to write performant code.  For this article, lets set aside Benchee as a dev tool and focus on how those use cases which involve ExUnit.

## Testing the Limits of the System
* Can your system handle 10x today's average load?
* Can a system with 1/2 the processing power handle the same load?  What are the impacts?
* How long does it take your system to process 100,000 login requests?  How about 100,000 comments?  How does that compare to processing 1,000 requests?

Answering these questions can help a team spend their time wisely.  You can identify where the problem spots in your code are using data, and let that drive development.  If your company has growth targets, you can write tests to assert that those minimums are met with different hardware or under adverse scenarios without having to live those nightmares.  Automating tests stretching your system not only prevents regressions but also ensures that your code isn't the bottleneck to growth for your company.

## Defining Performance Under Different Conditions

* Given situation A, the system can respond within a specified time?
* Given hardware B, is the system still able to handle the load?  Does it have enough memory allocated?
* In a critical situation, what exactly would this system do?  How long would that take?

## Automating Benchee Tests

Benchee tests don't play nicely with other tests, but with ExUnit tags, this can easily be avoided.  I would recommend creating a test suite dedicated to benchmarking functions and use this test suite to gate releases.  Depending on your needs, the test setup can be simple or complex. Simple cases testing standalone functions, asserting to outputs, and moving on to the next test is what Benchee is meant to do.  If you have a need to for more integration tests, Benchee can still be useful for integration cases but can be more difficult for reasons covered later in this post.

### Basic Unit Test Case

The usage examples in the documentation are usage examples for unit tests.  Since discovering Benchee, my workflow io to install a basic, happy path Benchee test around a given function once the function passes tests around functionality.  Now I can tinker with crucial aspects of the logic which might be slowing down processing.  For example, I might change an `Enum.each` and `Enum.map` into a single `Enum.reduce.`  For more integration test cases, metrics like deviation and 99th percentile cases can be valuable as well.

Since it doesn't show `ExUnit` integration in the docs, here is what a Benchee ExUnit test would look like:

```
defmodule Application.BencheeUnitTest do
    alias Application.HelperModule
    alias Application.TestHelper

    @tag :benchmark
    test "benchmark function a" do
        # setting output aside we can make some assertions about the output
        output = Benchee.run(%{
            "case_a" => fn(input, _resource) ->
                {:ok, result} = HelperModule.do_something(:case_a)
                # returning values can be used by after_each
                # if possible, its best to move assertions to the after_each
                # to avoid impacting tests
                result
            end,
            "case_b" => fn(_input, _resource) ->
                {:ok, result} = HelperModule.do_something(:case_b)
                result
            end,
            before_each: fn(input) ->
                # do any test setup here, and return input and test resources for EACH iteration
                {:ok, resources} = TestHelper.get_resources(input)

                # return tuple with input and the test resources
                {input, resources}
            end,
            after_each: fn(_result) ->
                # make assertions about the output of EACH iteration
            end
            # each iterates through inputs for each scenario
            inputs: %{
                "input_1" => 1,
                "input_2" => 2
            }
        })

        first_scenario = Enum.at(output.scenarios, 0)
        second_scenario = Enum.at(output.scenarios, 1)

        # make assertions of the performance, in micro seconds (this is 50ms)
        assert first_scenario.run_time_data.statistics.average <= 50_000_000
    end
end
```


## Integration Tests

One of the strengths of Elixir is the ability to build application trees using GenServers.  Unfortunately, these cases can often be challenging to test.  They require the processing of messages in a queue, and while you should aim only to test one module at a time, there are certain cases where you have to end to end test that GenServer B processed a message from GenServer A.  Worse still, Benchee scoping is not inherited from the test module.  Any setup you do must happen inside the before_each or inside the Benchee run itself.

For my use case, I found it helpful to create a module to handle events for tests.  If you have an event-based system, this might be the right approach.  The point of this is that we have some way of asserting that a process path has completed the moment it completes.  These assertions add time to each test run, but as long as it is apples to apples (comparing against past performance), we still get value out of these tests.

Here is an example of how that would look.

```
before_each: fn(input) ->
# inside your before_each, start a mock to handle events, passing this test pid as the callback
# this mock event handler has logic which passes along messages
    MockEventHandler.start_link([callback_pid: self()])
    EventHelper.subscribe_to_events(MockEventHandler, [:event_a, :event_a_completed])
...
```

Now, I can assert that events have been received to indicate a process completed inside the scenarios.

```
"case_a" => fn(input, resource) ->
    ...
    # this payload struct would be defined by your mock, here is an example
    assert_receive %{
        event: :event_a,
        # we can match this exactly, or pass the value to after_each to make assertions
        payload: payload
    }

    assert_receive %{
        event: :event_a_complete,
        payload: complete_payload
    }
    ...
end,
```

### Gotchas

* Scoping

Inside a benchmark run, you can access files within your application.  In our example above, imagine we had imported a Mock for our tests.  Benchee scoping does not inherit from the ExUnit case currently, so some tests require a great deal of setup and tear down.

* Iterations per second

In hindsight, it seems obvious, but initially, I did not understand that each benchee run ran as many times through a given chunk of code as possible. The before_each and after_each parameters refer to each iteration, not each scenario.

* Inputs and iterations

If given inputs, they too are cycled through in each iteration.  You can process these inputs in the before_each method and test scenarios for a given chunk of code.

