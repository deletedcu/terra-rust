# Terra Rust API
A rust API for Terrad's LCD system.

# Warning
This is a WIP.

No security audit has been performed.

## Randomness
The API is currently using random numbers via     
`
let mut rng = rand::thread_rng();
`
## Disclaimer

This may steal your money.

This is not investment advice.

Do your own research

# An Example
```rust
use terra_rust_api::{Terra, GasOptions, PrivateKey};
use terra_rust_api::core_types::{Coin, Msg, StdSignMsg, StdSignature};
use terra_rust_api::messages::MsgSend;
use terra_rust_api::auth_types::AuthAccountResult;

// set up the LCD client
let gas_opts = GasOptions::create_with_gas_estimate("50ukrw",1.4);
let t = Terra::lcd_client("https://tequila-lcd.terra.dev/", "tequila-0004", &gas_opts).await?;
// generate a private key
let secp = Secp256k1::new();
let from_key = PrivateKey::from_words(&secp,"your secret words");
let from_public_key = from_key.public_key(&secp);
// generate the message SEND 1000 uluna from your private key to someone else
let coin: Coin = Coin::parse("1000uluna")?.unwrap();
let from_account = from_public_key.account()?;
let send: MsgSend = MsgSend::create(from_account, "terra1usws7c2c6cs7nuc8vma9qzaky5pkgvm2uag6rh", vec![coin]);
// generate the transaction & calc fees
let messages = vec![send];
// and submit the message(s) to the chain
let resp = terra.submit_transaction_sync(
                    &secp,
                    &from_key,
                    &messages,
                    None
                )
                .await?;
 println!("{}", resp.txhash)
 
```

# Docs 
* [API Documentation](https://docs.rs/terra-rust-api)