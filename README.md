# mauth-protocol-test-suite

This repo contains test cases for mAuth digital signature protocols and was created to aid in the development of mAuth clients in a language agnostic way. Currently the repo only contains cases for the MWSV2 protocol as described [here](https://learn.mdsol.com/display/CA/MAuth+Protocol+V2+Specification). The repo also serves as an description of mAuth protocol specifications with examples.

## Usage

This repo should be added to each mAuth client via git submodules (see documentation [here](https://git-scm.com/book/en/v2/Git-Tools-Submodules) and run as part of the test suite for that mAuth client to ensure it conforms to the mAuth protocol specification. mAuth clients are expected to write some glue code that will allow them to run the cases provided here with their testing tool (Rspec etc).

### Test Cases

For each there are four files with the following extensions: `.req`, `.sts`, `.sig`, `.authz`.

The `.req` files contain a JSON hash of the attributes of an unsigned request.
The `.sts` files contain the `string_to_sign` (the string that will be passed through mAuth client's hashing algorithm) for that request.
The `.sig` files contain the digital signature of that request.
The `.authz` files contain a JSON hash of the authentication headers that would be added to that request in order to sign it.

For each case, clients that sign requests should run three tests:
1. Given the request attributes in the `.req` file, the client should generate a string_to_sign that matches the `.sts` file.
2. Given the string_to_sign in the `.sts` file, the client should generate a digital signature that matches the `.sig` file.
3. Given the signature in the `.sig` file, the client should generate authentication headers that match the headers in the `.authz` file.

Clients that authenticate requests should also run an additional test:
1. Combining the authentication headers in the `.authz` file and the request attributes in the `.req` file into a signed request, the client should consider the request authentic.

### Signing Parameters

The mAuth client running these tests should sign requests with the [provided RSA private key](./signing-params/rsa-key) and authenticate requests with the [provided RSA public key](./signing-params/rsa-key-pub). All requests should be signed and authenticated with the `app_uuid` and `request_time` provided in [signing-config.json](./signing-config.json). If the testing mAuth client does not accept the request time as an argument some library that mocks time APIs (i.e. Timecop for Ruby) should be used.