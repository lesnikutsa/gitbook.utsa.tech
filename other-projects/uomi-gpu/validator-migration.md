# 📬 Validator migration

At this time, the best thing to do is to transfer the keystore directory to the new server. Before transferring, stop the old server to prevent slashing

***

For the upcoming Turing testnet, we're implementing two "chill" functions:

One in the staking pallet One in the uomi-engine pallet We will provide documentation on how to use these functions. The process will work as follows:

Activate the chill functions Wait for all queued requests to be processed Then you can safely shut down your validator for a specified period (duration still being determined during development)

Of course when transferring the keystore directory to the new server, please take appropriate security precautions as it contains sensitive private keys
