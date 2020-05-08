# Types
## <a href="#crypto_errno" name="crypto_errno"></a> `crypto_errno`: Enum(`u16`)
Error codes.

Size: 2

Alignment: 2

### Variants
- <a href="#crypto_errno.success" name="crypto_errno.success"></a> `success`
Operation succeeded.

- <a href="#crypto_errno.guest_error" name="crypto_errno.guest_error"></a> `guest_error`
An error occurred when trying to during a conversion from a host type to a guest type.

Only an internal bug can throw this error.

- <a href="#crypto_errno.not_implemented" name="crypto_errno.not_implemented"></a> `not_implemented`
The requested operation is valid, but not implemented by the host.

- <a href="#crypto_errno.unsupported_feature" name="crypto_errno.unsupported_feature"></a> `unsupported_feature`
The requested feature is not supported by the chosen algorithm.

- <a href="#crypto_errno.prohibited_operation" name="crypto_errno.prohibited_operation"></a> `prohibited_operation`
The requested operation is valid, but was administratively prohibited.

- <a href="#crypto_errno.unsupported_encoding" name="crypto_errno.unsupported_encoding"></a> `unsupported_encoding`
Unsupported encoding for an import or export operation.

- <a href="#crypto_errno.unsupported_algorithm" name="crypto_errno.unsupported_algorithm"></a> `unsupported_algorithm`
The requested algorithm is not supported by the host.

- <a href="#crypto_errno.unsupported_option" name="crypto_errno.unsupported_option"></a> `unsupported_option`
The requested option is not supported by the currently selected algorithm.

- <a href="#crypto_errno.invalid_key" name="crypto_errno.invalid_key"></a> `invalid_key`
An invalid or incompatible key was supplied.

The key may not be valid, or was generated for a different algorithm or parameters set.

- <a href="#crypto_errno.invalid_length" name="crypto_errno.invalid_length"></a> `invalid_length`
The currently selected algorithm doesn't support the requested output length.

This error is thrown by non-extensible hash functions, when requesting an output size larger than they produce out of a single block.

- <a href="#crypto_errno.verification_failed" name="crypto_errno.verification_failed"></a> `verification_failed`
A signature or authentication tag verification failed.

- <a href="#crypto_errno.rng_error" name="crypto_errno.rng_error"></a> `rng_error`
A secure random numbers generator is not available.

The requested operation requires random numbers, but the host cannot securely generate them at the moment.

- <a href="#crypto_errno.algorithm_failure" name="crypto_errno.algorithm_failure"></a> `algorithm_failure`
An error was returned by the underlying cryptography library.

The host may be running out of memory, parameters may be incompatible with the chosen implementation of an algorithm or another unexpected error may have happened.

Ideally, the specification should provide enough details and guidance to make this error impossible to ever be thrown.

Realistically, the WASI crypto module cannot possibly cover all possible error types implementations can return, especially since some of these may be language-specific.
This error can thus be thrown when other error types are not suitable, and when the original error comes from the cryptographic primitives themselves and not from the WASI module.

- <a href="#crypto_errno.invalid_signature" name="crypto_errno.invalid_signature"></a> `invalid_signature`
The supplied signature is invalid, or incompatible with the chosen algorithm.

- <a href="#crypto_errno.closed" name="crypto_errno.closed"></a> `closed`
An attempt was made to close a handle that was already closed.

- <a href="#crypto_errno.invalid_handle" name="crypto_errno.invalid_handle"></a> `invalid_handle`
A function was called with an unassigned handle, a closed handle, or handle of an unexpected type.

- <a href="#crypto_errno.overflow" name="crypto_errno.overflow"></a> `overflow`
The host needs to copy data to a guest-allocated buffer, but that buffer is too small.

- <a href="#crypto_errno.internal_error" name="crypto_errno.internal_error"></a> `internal_error`
An internal error occurred.

This error is reserved to internal consistency checks, and must only be sent if the internal state of the host remains safe after an inconsistency was detected.

- <a href="#crypto_errno.too_many_handles" name="crypto_errno.too_many_handles"></a> `too_many_handles`
Too many handles are currently open, and a new one cannot be created.

Implementations are free to represent handles as they want, and to enforce limits to limit resources usage.

- <a href="#crypto_errno.key_not_supported" name="crypto_errno.key_not_supported"></a> `key_not_supported`
A key was provided, but the chosen algorithm doesn't support keys.

This is returned by symmetric operations.

Many hash functions, in particular, do not support keys without being used in particular constructions.
Blindly ignoring a key provided by mistake while trying to open a context for such as function could cause serious security vulnerabilities.

These functions must refuse to create the context and return this error instead.

- <a href="#crypto_errno.key_required" name="crypto_errno.key_required"></a> `key_required`
A key is required for the chosen algorithm, but none was given.

- <a href="#crypto_errno.invalid_tag" name="crypto_errno.invalid_tag"></a> `invalid_tag`
The provided authentication tag is invalid or incompatible with the current algorithm.

This error is returned by decryption functions and tag verification functions.

Unlike `verification_failed`, this error code is returned when the tag cannot possibly verify for any input.

- <a href="#crypto_errno.invalid_operation" name="crypto_errno.invalid_operation"></a> `invalid_operation`
The requested operation is incompatible with the current scheme.

For example, the `symmetric_state_encrypt()` function cannot complete if the selected construction is a key derivation function.
This error code will be returned instead.

- <a href="#crypto_errno.nonce_required" name="crypto_errno.nonce_required"></a> `nonce_required`
A nonce is required.

Most encryption schemes require a nonce.

In the absence of a nonce, the WASI cryptography module can automatically generate one, if that can be done safely. The nonce can be retrieved later with the `symmetric_state_option_get()` function using the `nonce` parameter.
If automatically generating a nonce cannot be done safely, the module never falls back to an insecure option and requests an explicit nonce by throwing that error.

- <a href="#crypto_errno.option_not_set" name="crypto_errno.option_not_set"></a> `option_not_set`
The named option was not set.

The caller tried to read the value of an option that was not set.
This error is used to make the distinction between an empty option, and an option that was not set and left to its default value.

- <a href="#crypto_errno.key_not_found" name="crypto_errno.key_not_found"></a> `key_not_found`
A key or key pair matching the requested identifier cannot be found using the supplied information.

This error is returned by a key manager via the `signature_keypair_from_id()` function.

- <a href="#crypto_errno.parameters_missing" name="crypto_errno.parameters_missing"></a> `parameters_missing`
The algorithm requires parameters that haven't been set.

Non-generic options are required and must be given by building an [`options`](#options) set and giving that object to functions instantiating that algorithm.

- <a href="#crypto_errno.in_progress" name="crypto_errno.in_progress"></a> `in_progress`
A requested computation is not done yet, and additional calls to the function are required.

Some functions, such as functions generating key pairs and password stretching functions, can take a long time to complete.

In order to avoid a host call to be blocked for too long, these functions can return prematurely, requiring additional calls with the same parameters until they complete.

## <a href="#keypair_encoding" name="keypair_encoding"></a> `keypair_encoding`: Enum(`u16`)
Encoding to use for importing or exporting a key pair.

Size: 2

Alignment: 2

### Variants
- <a href="#keypair_encoding.raw" name="keypair_encoding.raw"></a> `raw`
Raw bytes.

- <a href="#keypair_encoding.pkcs8" name="keypair_encoding.pkcs8"></a> `pkcs8`
PCSK8 encoding.

- <a href="#keypair_encoding.der" name="keypair_encoding.der"></a> `der`
DER encoding.

- <a href="#keypair_encoding.pem" name="keypair_encoding.pem"></a> `pem`
PEM encoding.

## <a href="#publickey_encoding" name="publickey_encoding"></a> `publickey_encoding`: Enum(`u16`)
Encoding to use for importing or exporting a public key.

Size: 2

Alignment: 2

### Variants
- <a href="#publickey_encoding.raw" name="publickey_encoding.raw"></a> `raw`
Raw bytes.

- <a href="#publickey_encoding.der" name="publickey_encoding.der"></a> `der`
DER encoding.

- <a href="#publickey_encoding.pem" name="publickey_encoding.pem"></a> `pem`
PEM encoding.

- <a href="#publickey_encoding.sec" name="publickey_encoding.sec"></a> `sec`
SEC encoding.

- <a href="#publickey_encoding.compressed_sec" name="publickey_encoding.compressed_sec"></a> `compressed_sec`
Compressed SEC encoding.

## <a href="#signature_encoding" name="signature_encoding"></a> `signature_encoding`: Enum(`u16`)
Encoding to use for importing or exporting a signature.

Size: 2

Alignment: 2

### Variants
- <a href="#signature_encoding.raw" name="signature_encoding.raw"></a> `raw`
Raw bytes.

- <a href="#signature_encoding.der" name="signature_encoding.der"></a> `der`
DER encoding.

## <a href="#options_type" name="options_type"></a> `options_type`: Enum(`u16`)
Type of an options set.

This is used when creating a new options set with `options_open()`.

Size: 2

Alignment: 2

### Variants
- <a href="#options_type.signatures" name="options_type.signatures"></a> `signatures`

- <a href="#options_type.symmetric" name="options_type.symmetric"></a> `symmetric`

## <a href="#version" name="version"></a> `version`: Int(`u64`)
Version of a managed key.

A version can be an arbitrary `u64` integer, with the expection of some reserved values.

Size: 8

Alignment: 8

### Consts
- <a href="#version.unspecified" name="version.unspecified"></a> `unspecified`
Key doesn't support versioning.

- <a href="#version.latest" name="version.latest"></a> `latest`
Use the latest version of a key.

- <a href="#version.all" name="version.all"></a> `all`
Perform an operation over all versions of a key.

## <a href="#size" name="size"></a> `size`: `usize`
Size of a value.

Size: 4

Alignment: 4

## <a href="#array_output" name="array_output"></a> `array_output`
Handle for functions returning output whose size may be large or not known in advance.

An [`array_output`](#array_output) object contains a host-allocated byte array.

A guest can get the size of that array after a function returns in order to then allocate a buffer of the correct size.
In addition, the content of such an object can be consumed by a guest in a streaming fashion.

An [`array_output`](#array_output) handle is automatically closed after its full content has been consumed.

Size: 4

Alignment: 4

### Supertypes
## <a href="#options" name="options"></a> `options`
A set of options.

This type is used to set non-default parameters.

The exact set of allowed options depends on the algorithm being used.

Size: 4

Alignment: 4

### Supertypes
## <a href="#key_manager" name="key_manager"></a> `key_manager`
A handle to the optional key management facilities offered by a host.

This is used to generate, retrieve and invalidate managed keys.

Size: 4

Alignment: 4

### Supertypes
## <a href="#signature_keypair" name="signature_keypair"></a> `signature_keypair`
A key pair for signatures.

Size: 4

Alignment: 4

### Supertypes
## <a href="#signature_state" name="signature_state"></a> `signature_state`
A state to absorb data to be signed.

After a signature has been computed or verified, the state remains valid for further operations.

A subsequent signature would sign all the data accumulated since the creation of the state object.

Size: 4

Alignment: 4

### Supertypes
## <a href="#signature" name="signature"></a> `signature`
A signature.

Size: 4

Alignment: 4

### Supertypes
## <a href="#signature_publickey" name="signature_publickey"></a> `signature_publickey`
A public key that can be used to verify a signature.

Size: 4

Alignment: 4

### Supertypes
## <a href="#signature_verification_state" name="signature_verification_state"></a> `signature_verification_state`
A state to absorb signed data to be verified.

Size: 4

Alignment: 4

### Supertypes
## <a href="#symmetric_state" name="symmetric_state"></a> `symmetric_state`
A state to perform symmetric operations.

The state is not reset nor invalidated after an option has been performed.
Incremental updates and sessions are thus supported.

Size: 4

Alignment: 4

### Supertypes
## <a href="#symmetric_key" name="symmetric_key"></a> `symmetric_key`
A symmetric key.

The key can be imported from raw bytes, or can be a reference to a managed key.

If it was imported, the host will wipe it from memory as soon as the handle is closed.

Size: 4

Alignment: 4

### Supertypes
## <a href="#symmetric_tag" name="symmetric_tag"></a> `symmetric_tag`
An authentication tag.

This is an object returned by functions computing authentication tags.

A tag can be compared against another tag (directly supplied as raw bytes) in constant time with the `symmetric_tag_verify()` function.

This object type can't be directly created from raw bytes. They are only returned by functions computing MACs.

The host is reponsible for securely wiping them from memory on close.

Size: 4

Alignment: 4

### Supertypes
## <a href="#opt_options_u" name="opt_options_u"></a> `opt_options_u`: Enum(`u8`)
Options index, only required by the Interface Types translation layer.

Size: 1

Alignment: 1

### Variants
- <a href="#opt_options_u.some" name="opt_options_u.some"></a> `some`

- <a href="#opt_options_u.none" name="opt_options_u.none"></a> `none`

## <a href="#opt_options" name="opt_options"></a> `opt_options`: Union
An optional options set.

This union simulates an `Option<Options>` type to make the [`options`](#options) parameter of some functions optional.

Size: 8

Alignment: 4

### Union Layout
- tag_size: 1
- tag_align: 1
- contents_offset: 4
- contents_size: 4
- contents_align: 4
### Union variants
- <a href="#opt_options.some" name="opt_options.some"></a> `some`: [`options`](#options)

- <a href="#opt_options.none" name="opt_options.none"></a> `none`

## <a href="#opt_symmetric_key_u" name="opt_symmetric_key_u"></a> `opt_symmetric_key_u`: Enum(`u8`)
Symmetric key index, only required by the Interface Types translation layer.

Size: 1

Alignment: 1

### Variants
- <a href="#opt_symmetric_key_u.some" name="opt_symmetric_key_u.some"></a> `some`

- <a href="#opt_symmetric_key_u.none" name="opt_symmetric_key_u.none"></a> `none`

## <a href="#opt_symmetric_key" name="opt_symmetric_key"></a> `opt_symmetric_key`: Union
An optional symmetric key.

This union simulates an `Option<SymmetricKey>` type to make the [`symmetric_key`](#symmetric_key) parameter of some functions optional.

Size: 8

Alignment: 4

### Union Layout
- tag_size: 1
- tag_align: 1
- contents_offset: 4
- contents_size: 4
- contents_align: 4
### Union variants
- <a href="#opt_symmetric_key.some" name="opt_symmetric_key.some"></a> `some`: [`symmetric_key`](#symmetric_key)

- <a href="#opt_symmetric_key.none" name="opt_symmetric_key.none"></a> `none`

# Modules
## <a href="#wasi_ephemeral_crypto_common" name="wasi_ephemeral_crypto_common"></a> wasi_ephemeral_crypto_common
### Imports
#### Memory
### Functions

---

#### <a href="#options_open" name="options_open"></a> `options_open(options_type: options_type) -> (crypto_errno, options)`
Create a new object to set non-default options.

Example usage:

```rust
let options_handle = options_open()?;
options_set(options_handle, "context", context)?;
options_set_u64(options_handle, "threads", 4)?;
let state = symmetric_state_open("BLAKE3", None, Some(options_handle))?;
options_close(options_handle)?;
```

##### Params
- <a href="#options_open.options_type" name="options_open.options_type"></a> `options_type`: [`options_type`](#options_type)

##### Results
- <a href="#options_open.error" name="options_open.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#options_open.handle" name="options_open.handle"></a> `handle`: [`options`](#options)


---

#### <a href="#options_close" name="options_close"></a> `options_close(handle: options) -> crypto_errno`
Destroy an options object.

Objects are reference counted. It is safe to close an object immediately after the last function needing it is called.

##### Params
- <a href="#options_close.handle" name="options_close.handle"></a> `handle`: [`options`](#options)

##### Results
- <a href="#options_close.error" name="options_close.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#options_set" name="options_set"></a> `options_set(handle: options, name: string, value: ConstPointer<u8>, value_len: size) -> crypto_errno`
Set or update an option.

This is used to set algorithm-specific parameters, but also to provide credentials for the key management facilities, if required.

This function may return `unsupported_option` if an option that doesn't exist for any implemented algorithms is specified.

##### Params
- <a href="#options_set.handle" name="options_set.handle"></a> `handle`: [`options`](#options)

- <a href="#options_set.name" name="options_set.name"></a> `name`: `string`

- <a href="#options_set.value" name="options_set.value"></a> `value`: `ConstPointer<u8>`

- <a href="#options_set.value_len" name="options_set.value_len"></a> `value_len`: [`size`](#size)

##### Results
- <a href="#options_set.error" name="options_set.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#options_set_u64" name="options_set_u64"></a> `options_set_u64(handle: options, name: string, value: u64) -> crypto_errno`
Set or update an integer option.

This is used to set algorithm-specific parameters.

This function may return `unsupported_option` if an option that doesn't exist for any implemented algorithms is specified.

##### Params
- <a href="#options_set_u64.handle" name="options_set_u64.handle"></a> `handle`: [`options`](#options)

- <a href="#options_set_u64.name" name="options_set_u64.name"></a> `name`: `string`

- <a href="#options_set_u64.value" name="options_set_u64.value"></a> `value`: `u64`

##### Results
- <a href="#options_set_u64.error" name="options_set_u64.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#options_set_guest_buffer" name="options_set_guest_buffer"></a> `options_set_guest_buffer(handle: options, name: string, buffer: Pointer<u8>, buffer_len: size) -> crypto_errno`
Set or update a guest-allocated memory that the host can use or return data into.

This is for example used to set the scratch buffer required by memory-hard functions.

This function may return `unsupported_option` if an option that doesn't exist for any implemented algorithms is specified.

##### Params
- <a href="#options_set_guest_buffer.handle" name="options_set_guest_buffer.handle"></a> `handle`: [`options`](#options)

- <a href="#options_set_guest_buffer.name" name="options_set_guest_buffer.name"></a> `name`: `string`

- <a href="#options_set_guest_buffer.buffer" name="options_set_guest_buffer.buffer"></a> `buffer`: `Pointer<u8>`

- <a href="#options_set_guest_buffer.buffer_len" name="options_set_guest_buffer.buffer_len"></a> `buffer_len`: [`size`](#size)

##### Results
- <a href="#options_set_guest_buffer.error" name="options_set_guest_buffer.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#array_output_len" name="array_output_len"></a> `array_output_len(array_output: array_output) -> (crypto_errno, size)`
Return the length of an [`array_output`](#array_output) object.

This allows a guest to allocate a buffer of the correct size in order to copy the output of a function returning this object type.

##### Params
- <a href="#array_output_len.array_output" name="array_output_len.array_output"></a> `array_output`: [`array_output`](#array_output)

##### Results
- <a href="#array_output_len.error" name="array_output_len.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#array_output_len.len" name="array_output_len.len"></a> `len`: [`size`](#size)


---

#### <a href="#array_output_pull" name="array_output_pull"></a> `array_output_pull(array_output: array_output, buf: Pointer<u8>, buf_len: size) -> (crypto_errno, size)`
Copy the content of an [`array_output`](#array_output) object into an application-allocated buffer.

Multiple calls to that function can be made in order to consume the data in a streaming fashion, if necessary.

The function returns the number of bytes that were actually copied. `0` means that the end of the stream has been reached. The total size always matches the output of `array_output_len()`.

The handle is automatically closed after all the data has been consumed.

Example usage:

```rust
let len = array_output_len(output_handle)?;
let mut out = vec![0u8; len];
array_output_pull(output_handle, &mut out)?;
```

##### Params
- <a href="#array_output_pull.array_output" name="array_output_pull.array_output"></a> `array_output`: [`array_output`](#array_output)

- <a href="#array_output_pull.buf" name="array_output_pull.buf"></a> `buf`: `Pointer<u8>`

- <a href="#array_output_pull.buf_len" name="array_output_pull.buf_len"></a> `buf_len`: [`size`](#size)

##### Results
- <a href="#array_output_pull.error" name="array_output_pull.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#array_output_pull.len" name="array_output_pull.len"></a> `len`: [`size`](#size)


---

#### <a href="#key_manager_open" name="key_manager_open"></a> `key_manager_open(options: opt_options) -> (crypto_errno, key_manager)`
__(optional)__
Create a context to use a key manager.

The set of required and supported options is defined by the host.

The function returns the `unsupported_feature` error code if key management facilities are not supported by the host.
This is also an optional import, meaning that the function may not even exist.

##### Params
- <a href="#key_manager_open.options" name="key_manager_open.options"></a> `options`: [`opt_options`](#opt_options)

##### Results
- <a href="#key_manager_open.error" name="key_manager_open.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#key_manager_open.handle" name="key_manager_open.handle"></a> `handle`: [`key_manager`](#key_manager)


---

#### <a href="#key_manager_close" name="key_manager_close"></a> `key_manager_close(key_manager: key_manager) -> crypto_errno`
__(optional)__
Destroy a key manager context.

The function returns the `unsupported_feature` error code if key management facilities are not supported by the host.
This is also an optional import, meaning that the function may not even exist.

##### Params
- <a href="#key_manager_close.key_manager" name="key_manager_close.key_manager"></a> `key_manager`: [`key_manager`](#key_manager)

##### Results
- <a href="#key_manager_close.error" name="key_manager_close.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#key_manager_invalidate" name="key_manager_invalidate"></a> `key_manager_invalidate(key_manager: key_manager, key_id: ConstPointer<u8>, key_id_len: size, key_version: version) -> crypto_errno`
__(optional)__
Invalidate a managed key or key pair given an identifier and a version.

This asks the key manager to delete or revoke a stored key, a specific version of a key..

`key_version` can be set to a version number, to [`version.latest`](#version.latest) to invalidate the current version, or to [`version.all`](#version.all) to invalidate all versions of a key.

The function returns `unsupported_feature` if this operation is not supported by the host, and `key_not_found` if the identifier and version don't match any existing key.

This is an optional import, meaning that the function may not even exist.

##### Params
- <a href="#key_manager_invalidate.key_manager" name="key_manager_invalidate.key_manager"></a> `key_manager`: [`key_manager`](#key_manager)

- <a href="#key_manager_invalidate.key_id" name="key_manager_invalidate.key_id"></a> `key_id`: `ConstPointer<u8>`

- <a href="#key_manager_invalidate.key_id_len" name="key_manager_invalidate.key_id_len"></a> `key_id_len`: [`size`](#size)

- <a href="#key_manager_invalidate.key_version" name="key_manager_invalidate.key_version"></a> `key_version`: [`version`](#version)

##### Results
- <a href="#key_manager_invalidate.error" name="key_manager_invalidate.error"></a> `error`: [`crypto_errno`](#crypto_errno)

## <a href="#wasi_ephemeral_crypto_signatures" name="wasi_ephemeral_crypto_signatures"></a> wasi_ephemeral_crypto_signatures
### Imports
#### Memory
### Functions

---

#### <a href="#signature_keypair_generate" name="signature_keypair_generate"></a> `signature_keypair_generate(algorithm: string, options: opt_options) -> (crypto_errno, signature_keypair)`
Generate a new key pair for signatures.

Internally, a key pair stores the supplied algorithm and optional parameters.

Trying to use that key pair with different parameters will throw an `invalid_key` error.

This function may return `$crypto_errno.unsupported_feature` if key generation is not supported by the host for the chosen algorithm.

The function may also return `unsupported_algorithm` if the algorithm is not supported by the host.

Finally, if generating that type of key pair is an expensive operation, the function may return `in_progress`.
In that case, the guest should retry with the same parameters until the function completes.

Example usage:

```rust
let kp_handle = ctx.signature_keypair_generate("RSA_PKCS1_2048_8192_SHA512", None)?;
```

##### Params
- <a href="#signature_keypair_generate.algorithm" name="signature_keypair_generate.algorithm"></a> `algorithm`: `string`

- <a href="#signature_keypair_generate.options" name="signature_keypair_generate.options"></a> `options`: [`opt_options`](#opt_options)

##### Results
- <a href="#signature_keypair_generate.error" name="signature_keypair_generate.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_keypair_generate.handle" name="signature_keypair_generate.handle"></a> `handle`: [`signature_keypair`](#signature_keypair)


---

#### <a href="#signature_keypair_import" name="signature_keypair_import"></a> `signature_keypair_import(algorithm: string, encoded: ConstPointer<u8>, encoded_len: size, encoding: keypair_encoding) -> (crypto_errno, signature_keypair)`
Import a key pair for signatures.

This function creates a [`signature_keypair`](#signature_keypair) object from existing material.

It may return `unsupported_algorithm` if the encoding scheme is not supported, or `invalid_key` if the key cannot be decoded.

The function may also return `unsupported_algorithm` if the algorithm is not supported by the host.

Example usage:

```rust
let kp_handle = ctx.signature_keypair_import("RSA_PKCS1_2048_8192_SHA512", KeypairEncoding::PKCS8)?;
```

##### Params
- <a href="#signature_keypair_import.algorithm" name="signature_keypair_import.algorithm"></a> `algorithm`: `string`

- <a href="#signature_keypair_import.encoded" name="signature_keypair_import.encoded"></a> `encoded`: `ConstPointer<u8>`

- <a href="#signature_keypair_import.encoded_len" name="signature_keypair_import.encoded_len"></a> `encoded_len`: [`size`](#size)

- <a href="#signature_keypair_import.encoding" name="signature_keypair_import.encoding"></a> `encoding`: [`keypair_encoding`](#keypair_encoding)

##### Results
- <a href="#signature_keypair_import.error" name="signature_keypair_import.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_keypair_import.handle" name="signature_keypair_import.handle"></a> `handle`: [`signature_keypair`](#signature_keypair)


---

#### <a href="#signature_managed_keypair_generate" name="signature_managed_keypair_generate"></a> `signature_managed_keypair_generate(key_manager: key_manager, algorithm: string, options: opt_options) -> (crypto_errno, signature_keypair)`
__(optional)__
Generate a new managed key pair.

The key pair is generated and stored by the key management facilities.

It may be used through its identifier, but the host may not allow it to be exported.

The function returns the `unsupported_feature` error code if key management facilities are not supported by the host,
or `unsupported_algorithm` if a key cannot be created for the chosen algorithm.

The function may also return `unsupported_algorithm` if the algorithm is not supported by the host.

This is also an optional import, meaning that the function may not even exist.

##### Params
- <a href="#signature_managed_keypair_generate.key_manager" name="signature_managed_keypair_generate.key_manager"></a> `key_manager`: [`key_manager`](#key_manager)

- <a href="#signature_managed_keypair_generate.algorithm" name="signature_managed_keypair_generate.algorithm"></a> `algorithm`: `string`

- <a href="#signature_managed_keypair_generate.options" name="signature_managed_keypair_generate.options"></a> `options`: [`opt_options`](#opt_options)

##### Results
- <a href="#signature_managed_keypair_generate.error" name="signature_managed_keypair_generate.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_managed_keypair_generate.handle" name="signature_managed_keypair_generate.handle"></a> `handle`: [`signature_keypair`](#signature_keypair)


---

#### <a href="#signature_keypair_id" name="signature_keypair_id"></a> `signature_keypair_id(kp: signature_keypair, kp_id: Pointer<u8>, kp_id_max_len: size) -> (crypto_errno, size, version)`
__(optional)__
Return the key pair identifier and version of a managed signature key pair.

If the key pair is not managed, `unsupported_feature` is returned instead.

This is an optional import, meaning that the function may not even exist.

##### Params
- <a href="#signature_keypair_id.kp" name="signature_keypair_id.kp"></a> `kp`: [`signature_keypair`](#signature_keypair)

- <a href="#signature_keypair_id.kp_id" name="signature_keypair_id.kp_id"></a> `kp_id`: `Pointer<u8>`

- <a href="#signature_keypair_id.kp_id_max_len" name="signature_keypair_id.kp_id_max_len"></a> `kp_id_max_len`: [`size`](#size)

##### Results
- <a href="#signature_keypair_id.error" name="signature_keypair_id.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_keypair_id.kp_id_len" name="signature_keypair_id.kp_id_len"></a> `kp_id_len`: [`size`](#size)

- <a href="#signature_keypair_id.version" name="signature_keypair_id.version"></a> `version`: [`version`](#version)


---

#### <a href="#signature_keypair_from_id" name="signature_keypair_from_id"></a> `signature_keypair_from_id(key_manager: key_manager, kp_id: ConstPointer<u8>, kp_id_len: size, kp_version: version) -> (crypto_errno, signature_keypair)`
__(optional)__
Return a managed signature key pair from a key identifier.

`kp_version` can be set to `version_latest` to retrieve the most recent version of a key pair.

If no key pair matching the provided information is found, `key_not_found` is returned instead.

This is an optional import, meaning that the function may not even exist.
``
##### Params
- <a href="#signature_keypair_from_id.key_manager" name="signature_keypair_from_id.key_manager"></a> `key_manager`: [`key_manager`](#key_manager)

- <a href="#signature_keypair_from_id.kp_id" name="signature_keypair_from_id.kp_id"></a> `kp_id`: `ConstPointer<u8>`

- <a href="#signature_keypair_from_id.kp_id_len" name="signature_keypair_from_id.kp_id_len"></a> `kp_id_len`: [`size`](#size)

- <a href="#signature_keypair_from_id.kp_version" name="signature_keypair_from_id.kp_version"></a> `kp_version`: [`version`](#version)

##### Results
- <a href="#signature_keypair_from_id.error" name="signature_keypair_from_id.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_keypair_from_id.handle" name="signature_keypair_from_id.handle"></a> `handle`: [`signature_keypair`](#signature_keypair)


---

#### <a href="#signature_keypair_export" name="signature_keypair_export"></a> `signature_keypair_export(kp: signature_keypair, encoding: keypair_encoding) -> (crypto_errno, array_output)`
Export a signature key pair as the given encoding format.

May return `prohibited_operation` if this operation is denied or `unsupported_encoding` if the encoding is not supported.

##### Params
- <a href="#signature_keypair_export.kp" name="signature_keypair_export.kp"></a> `kp`: [`signature_keypair`](#signature_keypair)

- <a href="#signature_keypair_export.encoding" name="signature_keypair_export.encoding"></a> `encoding`: [`keypair_encoding`](#keypair_encoding)

##### Results
- <a href="#signature_keypair_export.error" name="signature_keypair_export.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_keypair_export.encoded" name="signature_keypair_export.encoded"></a> `encoded`: [`array_output`](#array_output)


---

#### <a href="#signature_keypair_publickey" name="signature_keypair_publickey"></a> `signature_keypair_publickey(kp: signature_keypair) -> (crypto_errno, signature_publickey)`
Get a public key of a signature key pair.

The returned object can be used to verify signatures.

##### Params
- <a href="#signature_keypair_publickey.kp" name="signature_keypair_publickey.kp"></a> `kp`: [`signature_keypair`](#signature_keypair)

##### Results
- <a href="#signature_keypair_publickey.error" name="signature_keypair_publickey.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_keypair_publickey.pk" name="signature_keypair_publickey.pk"></a> `pk`: [`signature_publickey`](#signature_publickey)


---

#### <a href="#signature_keypair_close" name="signature_keypair_close"></a> `signature_keypair_close(kp: signature_keypair) -> crypto_errno`
Destroys a signature key pair.

The host will automatically wipe traces of the secret key from memory.

If this is a managed key, the key will not be removed from persistent storage, and can be reconstructed later using the key identifier.

##### Params
- <a href="#signature_keypair_close.kp" name="signature_keypair_close.kp"></a> `kp`: [`signature_keypair`](#signature_keypair)

##### Results
- <a href="#signature_keypair_close.error" name="signature_keypair_close.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_publickey_import" name="signature_publickey_import"></a> `signature_publickey_import(algorithm: string, encoded: ConstPointer<u8>, encoded_len: size, encoding: publickey_encoding) -> (crypto_errno, signature_publickey)`
Import a signature public key.

The returned object can be used to verify signatures.

The function may return `unsupported_encoding` if importing from the given format is not implemented or incompatible with the key type.

It may also return `invalid_key` if the key doesn't appear to match the supplied algorithm.

Finally, the function may return `unsupported_algorithm` if the algorithm is not supported by the host.

Example usage:

```rust
let pk_handle = ctx.signature_publickey_import(encoded, PublicKeyEncoding::Sec)?;
```

##### Params
- <a href="#signature_publickey_import.algorithm" name="signature_publickey_import.algorithm"></a> `algorithm`: `string`

- <a href="#signature_publickey_import.encoded" name="signature_publickey_import.encoded"></a> `encoded`: `ConstPointer<u8>`

- <a href="#signature_publickey_import.encoded_len" name="signature_publickey_import.encoded_len"></a> `encoded_len`: [`size`](#size)

- <a href="#signature_publickey_import.encoding" name="signature_publickey_import.encoding"></a> `encoding`: [`publickey_encoding`](#publickey_encoding)

##### Results
- <a href="#signature_publickey_import.error" name="signature_publickey_import.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_publickey_import.pk" name="signature_publickey_import.pk"></a> `pk`: [`signature_publickey`](#signature_publickey)


---

#### <a href="#signature_publickey_export" name="signature_publickey_export"></a> `signature_publickey_export(pk: signature_publickey, encoding: publickey_encoding) -> (crypto_errno, array_output)`
Export a public key as the given encoding format.

May return `unsupported_encoding` if the encoding is not supported.

##### Params
- <a href="#signature_publickey_export.pk" name="signature_publickey_export.pk"></a> `pk`: [`signature_publickey`](#signature_publickey)

- <a href="#signature_publickey_export.encoding" name="signature_publickey_export.encoding"></a> `encoding`: [`publickey_encoding`](#publickey_encoding)

##### Results
- <a href="#signature_publickey_export.error" name="signature_publickey_export.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_publickey_export.encoded" name="signature_publickey_export.encoded"></a> `encoded`: [`array_output`](#array_output)


---

#### <a href="#signature_publickey_verify" name="signature_publickey_verify"></a> `signature_publickey_verify(pk: signature_publickey) -> crypto_errno`
Check that a signature public key is valid and in canonical form.

This function may perform stricter checks than those made during importation at the expense of additional CPU cycles.

The function returns `invalid_key` if the public key didn't pass the checks.

##### Params
- <a href="#signature_publickey_verify.pk" name="signature_publickey_verify.pk"></a> `pk`: [`signature_publickey`](#signature_publickey)

##### Results
- <a href="#signature_publickey_verify.error" name="signature_publickey_verify.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_publickey_close" name="signature_publickey_close"></a> `signature_publickey_close(pk: signature_publickey) -> crypto_errno`
Destroys a public key.

Objects are reference counted. It is safe to close an object immediately after the last function needing it is called.

##### Params
- <a href="#signature_publickey_close.pk" name="signature_publickey_close.pk"></a> `pk`: [`signature_publickey`](#signature_publickey)

##### Results
- <a href="#signature_publickey_close.error" name="signature_publickey_close.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_export" name="signature_export"></a> `signature_export(signature: signature, encoding: signature_encoding) -> (crypto_errno, array_output)`
Export a signature.

This function exports a signature object using the specified encoding.

May return `unsupported_encoding` if the signature cannot be encoded into the given format.

##### Params
- <a href="#signature_export.signature" name="signature_export.signature"></a> `signature`: [`signature`](#signature)

- <a href="#signature_export.encoding" name="signature_export.encoding"></a> `encoding`: [`signature_encoding`](#signature_encoding)

##### Results
- <a href="#signature_export.error" name="signature_export.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_export.encoded" name="signature_export.encoded"></a> `encoded`: [`array_output`](#array_output)


---

#### <a href="#signature_import" name="signature_import"></a> `signature_import(algorithm: string, encoding: signature_encoding, encoded: ConstPointer<u8>, encoded_len: size) -> (crypto_errno, signature)`
Create a signature object.

This object can be used along with a public key to verify an existing signature.

It may return `invalid_signature` if the signature is invalid or incompatible with the specified algorithm, as well as `unsupported_encoding` if the encoding is not compatible with the signature type.

The function may also return `unsupported_algorithm` if the algorithm is not supported by the host.

Example usage:

```rust
let signature_handle = ctx.signature_import("ECDSA_P256_SHA256", SignatureEncoding::DER, encoded)?;
```

##### Params
- <a href="#signature_import.algorithm" name="signature_import.algorithm"></a> `algorithm`: `string`

- <a href="#signature_import.encoding" name="signature_import.encoding"></a> `encoding`: [`signature_encoding`](#signature_encoding)

- <a href="#signature_import.encoded" name="signature_import.encoded"></a> `encoded`: `ConstPointer<u8>`

- <a href="#signature_import.encoded_len" name="signature_import.encoded_len"></a> `encoded_len`: [`size`](#size)

##### Results
- <a href="#signature_import.error" name="signature_import.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_import.signature" name="signature_import.signature"></a> `signature`: [`signature`](#signature)


---

#### <a href="#signature_state_open" name="signature_state_open"></a> `signature_state_open(kp: signature_keypair) -> (crypto_errno, signature_state)`
Create a new state to collect data to compute a signature on.

This function allows data to be signed to be supplied in a streaming fashion.

The state is not closed and can be used after a signature has been computed, allowing incremental updates by calling `signature_state_update()` again afterwards.

Example usage - signature creation

```rust
let kp_handle = ctx.signature_keypair_import("Ed25519ph", keypair, KeypairEncoding::Raw)?;
let state_handle = ctx.signature_state_open(kp_handle)?;
ctx.signature_state_update(state_handle, b"message part 1")?;
ctx.signature_state_update(state_handle, b"message part 2")?;
let sig_handle = ctx.signature_state_sign(state_handle)?;
let raw_sig = ctx.signature_export(sig_handle, SignatureEncoding::Raw)?;
```

##### Params
- <a href="#signature_state_open.kp" name="signature_state_open.kp"></a> `kp`: [`signature_keypair`](#signature_keypair)

##### Results
- <a href="#signature_state_open.error" name="signature_state_open.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_state_open.state" name="signature_state_open.state"></a> `state`: [`signature_state`](#signature_state)


---

#### <a href="#signature_state_update" name="signature_state_update"></a> `signature_state_update(state: signature_state, input: ConstPointer<u8>, input_len: size) -> crypto_errno`
Absorb data into the signature state.

This function may return `unsupported_feature` is the selected algorithm doesn't support incremental updates.

##### Params
- <a href="#signature_state_update.state" name="signature_state_update.state"></a> `state`: [`signature_state`](#signature_state)

- <a href="#signature_state_update.input" name="signature_state_update.input"></a> `input`: `ConstPointer<u8>`

- <a href="#signature_state_update.input_len" name="signature_state_update.input_len"></a> `input_len`: [`size`](#size)

##### Results
- <a href="#signature_state_update.error" name="signature_state_update.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_state_sign" name="signature_state_sign"></a> `signature_state_sign(state: signature_state) -> (crypto_errno, array_output)`
Compute a signature for all the data collected up to that point.

The function can be called multiple times for incremental signing.

##### Params
- <a href="#signature_state_sign.state" name="signature_state_sign.state"></a> `state`: [`signature_state`](#signature_state)

##### Results
- <a href="#signature_state_sign.error" name="signature_state_sign.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_state_sign.signature" name="signature_state_sign.signature"></a> `signature`: [`array_output`](#array_output)


---

#### <a href="#signature_state_close" name="signature_state_close"></a> `signature_state_close(state: signature_state) -> crypto_errno`
Destroy a signature state.

Objects are reference counted. It is safe to close an object immediately after the last function needing it is called.

Note that closing a signature state doesn't close or invalidate the key pair object, that be reused for further signatures.

##### Params
- <a href="#signature_state_close.state" name="signature_state_close.state"></a> `state`: [`signature_state`](#signature_state)

##### Results
- <a href="#signature_state_close.error" name="signature_state_close.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_verification_state_open" name="signature_verification_state_open"></a> `signature_verification_state_open(kp: signature_publickey) -> (crypto_errno, signature_verification_state)`
Create a new state to collect data to verify a signature on.

This is the verification counterpart of [`signature_state`](#signature_state).

Data can be injected using `signature_verification_state_update()`, and the state is not closed after a verification, allowing incremental verification.

Example usage - signature verification:

```rust
let pk_handle = ctx.signature_publickey_import("ECDSA_P256_SHA256", encoded_pk PublicKeyEncoding::CompressedSec)?;
let signature_handle = ctx.signature_import("ECDSA_P256_SHA256", encoded_sig, PublicKeyEncoding::Der)?;
let state_handle = ctx.signature_verification_state_open(pk_handle)?;
ctx.signature_verification_state_update(state_handle, "message")?;
ctx.signature_verification_state_verify(signature_handle)?;
```

##### Params
- <a href="#signature_verification_state_open.kp" name="signature_verification_state_open.kp"></a> `kp`: [`signature_publickey`](#signature_publickey)

##### Results
- <a href="#signature_verification_state_open.error" name="signature_verification_state_open.error"></a> `error`: [`crypto_errno`](#crypto_errno)

- <a href="#signature_verification_state_open.state" name="signature_verification_state_open.state"></a> `state`: [`signature_verification_state`](#signature_verification_state)


---

#### <a href="#signature_verification_state_update" name="signature_verification_state_update"></a> `signature_verification_state_update(state: signature_verification_state, input: ConstPointer<u8>, input_len: size) -> crypto_errno`
Absorb data into the signature verification state.

This function may return `unsupported_feature` is the selected algorithm doesn't support incremental updates.

##### Params
- <a href="#signature_verification_state_update.state" name="signature_verification_state_update.state"></a> `state`: [`signature_verification_state`](#signature_verification_state)

- <a href="#signature_verification_state_update.input" name="signature_verification_state_update.input"></a> `input`: `ConstPointer<u8>`

- <a href="#signature_verification_state_update.input_len" name="signature_verification_state_update.input_len"></a> `input_len`: [`size`](#size)

##### Results
- <a href="#signature_verification_state_update.error" name="signature_verification_state_update.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_verification_state_verify" name="signature_verification_state_verify"></a> `signature_verification_state_verify(state: signature_verification_state, signature: signature) -> crypto_errno`
Check that the given signature is verifies for the data collected up to that point point.

The state is not closed and can absorb more data to allow for incremental verification.

The function returns `invalid_signature` if the signature doesn't appear to be valid.

##### Params
- <a href="#signature_verification_state_verify.state" name="signature_verification_state_verify.state"></a> `state`: [`signature_verification_state`](#signature_verification_state)

- <a href="#signature_verification_state_verify.signature" name="signature_verification_state_verify.signature"></a> `signature`: [`signature`](#signature)

##### Results
- <a href="#signature_verification_state_verify.error" name="signature_verification_state_verify.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_verification_state_close" name="signature_verification_state_close"></a> `signature_verification_state_close(state: signature_verification_state) -> crypto_errno`
Destroy a signature verification state.

Objects are reference counted. It is safe to close an object immediately after the last function needing it is called.

Note that closing a signature state doesn't close or invalidate the public key object, that be reused for further verifications.

##### Params
- <a href="#signature_verification_state_close.state" name="signature_verification_state_close.state"></a> `state`: [`signature_verification_state`](#signature_verification_state)

##### Results
- <a href="#signature_verification_state_close.error" name="signature_verification_state_close.error"></a> `error`: [`crypto_errno`](#crypto_errno)


---

#### <a href="#signature_close" name="signature_close"></a> `signature_close(signature: signature) -> crypto_errno`
Destroy a signature.

Objects are reference counted. It is safe to close an object immediately after the last function needing it is called.

##### Params
- <a href="#signature_close.signature" name="signature_close.signature"></a> `signature`: [`signature`](#signature)

##### Results
- <a href="#signature_close.error" name="signature_close.error"></a> `error`: [`crypto_errno`](#crypto_errno)
