---
layout: post
title:  "Using Kerl and Kiex for Erlang and Elixir Version Management"
date:   2018-09-17 18:15:05 -0700
categories: technical
cover: "/assets/elixir.png"
---

Working with Phoenix/Elixir/Erlang has its benefits, mainly performance and ease of writing production ready code, but one drawback is the headache of managing Elixir and Erlang versions. These types of issues are impediments to more widespread adoption, so I wanted to briefly document how to manage versions using kerl and kiex (Ubuntu 16.04).

![Elixir](/assets/elixir.png)

Once you understand some basic principles it isn’t too confusing.

* You should update your erlang version first, since you will need the updated erlang version to compile Elixir.
* Once you have installed and build different versions, you need to activate them.

[Github for Kerl](https://github.com/kerl/kerl)

[Github for Kiex](https://github.com/taylor/kiex)

For this tutorial I will be upgrading to erlang 21.0 and elixir 1.7.3.

```
# update the available releases
kerl update releases

# build is a fetch and compile
# build erlang version 21.0 named 21.0
kerl build 21.0 21.0

# this will take 20-30 minutes
# confirm the file path to install
kerl list installations

# install build name to file path, should be quick
kerl install 21.0 /home/david/kerl/21.0
-> Installing Erlang/OTP 21.0 (21.0) in /home/david/kerl/21.0...
You can activate this installation running the following command:
. /home/david/kerl/21.0/activate
Later on, you can leave the installation typing:
kerl_deactivate

# follow instructions to activate new erlang version
. /home/david/kerl/21.0/activate

# now, typing erl shows me the machine is using 21.0
erl
-> Erlang/OTP 21 [erts-10.0] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [hipe]
```

Great, erlang 21.0 is ready to use. Now for elixir.

```
# to see what versions are available
kiex list
kiex install 1.7.3
kiex use 1.7.3

# confirm the correct elixir version and erlang compile version
elixir --version
-> Erlang/OTP 21 [erts-8.1] [source] [64-bit] [smp:8:8] [async-threads:10] [hipe] [kernel-poll:false]
Elixir 1.7.3 (compiled with OTP 21)
```

If you continue to set versions and feel they are not saving, you may have incorrect defaults set in your bash profile or bash startup files.

Hopefully this helps someone save some time. Upgrades in Elixir are challenging enough without having to worry about whether you are using the right version of erlang.
