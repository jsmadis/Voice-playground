# Dialing

In the previous chapter (local setup) we set up asterisk and we configured 2 devices that are able to register. In this chapter we will:
- Extend the configuration to run some logic when one of the devices starts a call
- Explain how you can configure and "code" the behaviour of Asterisk.


## Setup explanation

### Configuration
We will reuse the same configuration as we used in the previous chapter. If feel lost by the configuration please read the previous chapter.


#### extensions.conf

Extensions file define contexts (or in programming terminology we can call it a function) that is executed when asterisk receives an input. For example, if a registered device wants to call another device.

In this chapter we will try to make call from device A to device B. Therefore we need to define a extension that will be called.

We define 2 contexts, one for device 6001 and another one for device 6002.

```conf
[6001-default]
exten => 1000,1,Noop(Executing 6001-default context)
same => n,Dial(PJSIP/6002)
same => n,Hangup()

[6002-default]
exten => 1000,1,Noop(Executing 6002-default context)
same => n,Dial(PJSIP/6001)
same => n,Hangup()
```

What it does?

`exten => 1000,1,Noop(Executing 6001-default context)`

When this context is executed with extension `1000` (we are calling number 1000) we will only log (`Noop()`) that we are executing this context.

`same => n,Dial(PJSIP/6002)`

The `Dial` function will try calling the device `6002`. It has a prefix PJSIP because we are using PJSIP as our sip engine.

`same => n,Hangup()`

The last line will make sure that the call will hangup in case of the dial doesn't go through.


#### pjsip.conf
We need modify `pjsip.conf` because we need to let Asterisk know what should be executed if connected device want's to make a call.

Since we have context (function) for each device we need to configure it for each device.
The keyword in the `pjsip.conf` is `context=` and we need to set it to `6001-default` and `6002-default`

Therefore we define the following

```conf
[6001](endpoint-basic)
auth=auth6001
aors=6001
context=6001-default
```

And

```conf
[6002](endpoint-basic)
auth=auth6002
aors=6002
context=6002-default
```