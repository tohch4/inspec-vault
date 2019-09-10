# inspec-vault Plugin

This is a plugin for [Chef InSpec](https://www.inspec.io/) that allows [Inputs](https://www.inspec.io/docs/reference/inputs/) to be read from [HashiCorp Vault](https://www.vaultproject.io/).

## To Install This Plugin

Assuming it has been published to RubyGems, you can install this gem using:

```
you@machine $ inspec plugin install inspec-vault
```

## What This Plugin Does

Whenever profile code like this is encountered:

```ruby
# In profile "my_profile"
describe input("some_input") do
  it { should cmp "some_expected_value" }
end
```

With no other settings, Chef InSpec will for a Vault secret located at `secret/inspec/my_profile` with a key "some_input". It will use the Vault if found; otherwise it will fall back to other means of resolving the input, such as the profile metadata or CLI values.

## Configuring the Plugin

Each plugin option may be set either as an environment variable, or as a plugin option in your Chef InSpec configuration file at `~/.inspec/config.json`. For example, to set the `prefix_path` option in the config file, lay out the config file as follows:

```json
{
  "version": "1.2",
  "plugins":{
    "inspec-vault":{
      "prefix_path":"my-profiles"
    }
  }
}
```

Config file option names are always lowercase.

This plugin supports the following options:

### INSPEC_VAULT_MOUNT_POINT

### mount_point

A string that indicates the where the key-value path should begin; default value is "secret". The path is constructed as `<mount_point>/data/<path_prefix>/<profile_name>`.

### INSPEC_VAULT_PATH_PREFIX

### path_prefix

A string that indicates the latter portion of the key-value path; default value is "inspec". The path is constructed as `<mount_point>/data/<path_prefix>/<profile_name>`.

### VAULT_ADDR

### vault_addr

This environment variable is the URL and port of your Vault installation. Default is http://127.0.0.1:8200.

### VAULT_TOKEN

This value is the secret used to authenticate to Vault. Required, no default provided.

## Developing This Plugin

Please have a look at our CONTRIBUTING.md for general guidelines.

### Testing

Run `bundle exec rake test:lint` for linting, `bundle exec rake test:unit` for unit tests, and `bundle exec rake test:integration` for integration tests.

Note that integration tests will download and run Vault server locally.

