This case for binary request bodies is slightly different than the other cases. mAuth clients running this test should read in the [blank.jpeg](./blank.jpeg) file as the request body.
The `.sts` file is intentionally omitted because the MWS protocol uses the binary request body itself as a part of the `string_to_sign`.
