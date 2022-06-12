# lambda-test

This here is me getting to grips with writing Rust code with AWS Lambda as the target execution environment.

## Getting started

Start a project as usual, with `cargo new lambda-test`

Add some suitable deps:
* lambda_runtime
* serde
* serde_json
* serde_derive
* log
* simple_logger

Add a ``[[bin]]`` section with the following key/values:  
`name = "bootstrap"`  
`path = "src/main.rs"`

The name here overrides the binary filename that will be created. AWS expects your binary to be called 'bootstrap'.

The AWS Lambda environment uses Amazon Linux / MUSL, so set a build target:  
`rustup target add x86_64-unknown-linux-musl`

(Note: if you are on MacOS, you can still build to this target but will need a musl cross-compiler - see <https://github.com/FiloSottile/homebrew-musl-cross>)

To build, run `cargo build --release --target x86_64-unknown-linux-musl`

To package, run `zip -j myRustFunction.zip ./target/x86_64-unknown-linux-musl/release/bootstrap`

Create a lambda function with 'Author from scratch' as the first option, and set the Runtime to 'Provide your own bootstrap on Amazon Linux 2'.

You can now use the 'Upload from .zip file' option in the Lambda console to upload your zip.

Create a suitable event, if required, and test away!  
This example uses a simple `{"firstName": "Andy"}` JSON event.
