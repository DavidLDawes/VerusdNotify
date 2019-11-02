# VerusdNotify
Handle verusd events using a go server

## What
Simple http server wrapper for the wallet and block events that you can have verusd generate.

verusd has command line options to run a command when wallet and block events occur. This code implements a go server that provides endpoints intended to be used via curl to handle those events.

## How
By passing in a curl command to hit the endpoint on the localhost, we can run the go command serving the endpoints on the local instance, then launch verusd using the command line options to add curl commands that send the notifications to our go endpoints when invoked by verusd as events occur.

Launch the go program - you can simply execute "go run VerusdEvents" in the src directory.
Next, when launching verusd, use the -blocknotify and -walletnotify command line options to include curl commands that send the passed in parameter (using %s) off to the TCP end point provided by the VerusdEvents.go program.
verus -blocknotify=`curl localhost:8080/block?blockhash=%s` -walletnotify=`curl localhost:8080/wallet?txid=%s`

## Version 0
Written in go, this app currently doesn't do anything except echo the input back as the response. That allows you to hit the end pont from a browser and see your input. Whee. Call this the version 0 implementation.

## Plan
First cut is to do some simple get operations to work out what the notification is referring to, reporting that in some manner.

Simple dumb test idea: test routines that wait for 1 of each notifications then render a pass verdict, or fail after N minutes without success, to use as a test tool in automated build testing.

I tend to like storing stuff behind JSON REST interfaces in DBs, so using something like JHipster to spin up data models, APIs, UIs and implementations would be reasonable. Using the RPC API wrapper library to get information and putting hte results into a reasonable model would be a useful sample.

In any event, the plan is to do useful work when the events fire at the very least to demonstrate proper operation and how-to steps for persisting the data would be useful.
## Credit
Asher Dawes discussed approaches to this sort of thing and went and began building out a go library. Asher pointed out the event stuff and asked about wiring it up somehow. This work addresses that thought directly, so Asher deserves co-creator credit for this at the very least.
# Copyright
[Copyright 2019 David Dawes & Asher Dawes - licensed under the MIT License](https://github.com/DavidLDawes/VerusdNotify/blob/master/LICENSE)
