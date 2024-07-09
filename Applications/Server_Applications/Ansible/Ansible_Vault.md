---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Ansible_Vault
## Introducing Ansible Vault

Ansible Vault can be used to encrypt any file, or variables themselves, from within a playbook. By default AES is used which is a shared-secret based encryption. Both file and variable encryption methods have their benefits and drawbacks.

### File Encryption

To create a new encrypted file named

```
secrets.yml
```

, simply use the following `ansible-vault` command.

```
ansible-vault create secrets.yml
```

After prompting for a password, the `ansible-vault` command will launch the default system file editor, which will result in an encrypted file upon saving.

Similarly, to encrypt a previously unencrypted file, use the following `ansible-vault` command. Note that this uses the `encrypt` parameter rather than the `create` parameter.

```
ansible-vault encrypt secrets.yml
```

The downside to using file encryption is readability. If you open the file then you will find that without decryption, it's impossible to decipher the contents.

### Variable Encryption

Within a playbook, it is possible to use an encrypted variable by prefacing the encrypted data with the `!vault` tag. Running the `encrypt_string` argument of the `ansible_vault` command will result in an encrypted string that you can use within your playbooks.

```
ansible-vault encrypt_string 'secret_data' --name 'my_secret'
```

After prompting you for a password, you will get the following encrypted string.

```
my_secret: !vault |          $ANSIBLE_VAULT;1.1;AES256          37636561366636643464376336303466613062633537323632306566653533383833366462366662          6565353063303065303831323539656138653863353230620a653638643639333133306331336365          62373737623337616130386137373461306535383538373162316263386165376131623631323434          3866363862363335620a376466656164383032633338306162326639643635663936623939666238          3161
```

Variable encryption is great for readability, but the ability to use command line rekeying is sacrificed when using this method.

## Using Ansible Vault in Practice

You may realize that using Ansible Vault in production is a challenge. To effectively use Ansible Vault, the following techniques make this process easier.

- Unprompted Decryption
- Multiple Vaults
- Rekeying

### Unprompted Decryption

One option to transparently decrypting a file or variable while using Ansible is to store the password within a protected and un-versioned password file. To reference this stored password, simply pass in the file location using the `vault-password-file` parameter.

```
ansible-playbook --vault-password-file /path/vault-password-file secrets.yml
```

This will decrypt any included encrypted files or variables using the included password.

It is very important not to commit your plaintext password file into your version control system. Similarly, protect this file to only the user or group that needs access to the stored password on the file system using access control lists (ACL's).

### Multiple Vaults

Although it's convenient to have a single vault with all of the encrypted secrets, a better security practice is to separate the secure credentials into separate relevant vaults. An example of this would be separating a production and development environment. Thankfully, Ansible Vault allows us to create multiple vaults and references which vault the encrypted data is coming from using a label.

```
ansible-vault create --vault-id prod@prompt prod-secrets.yml
```

The above code will create a `prod` vault and prompt for your password at runtime (as noted by the `@prompt` string). If you already have a password file that you would like to use, simply pass in the path to the file.

```
ansible-vault create --vault-id prod@/path/prod-vault-password-file prod-secrets.yml
```

Let's say we want to encrypt the same `my_secret` variable, but this time store that in our `prod` vault. Just as before, using `encrypt_string` but with the relevant `vault-id` allows storing of the secret in the specified location.

```
ansible-vault encrypt_string --vault-id prod@/path/prod-vault-password-file 'secret_data' --name 'my_secret'
```

You will notice that after the `AES256` string, a new piece of text, `prod` is shown. This indicates the vault that the encrypted text is located in.

```
my_secret: !vault |          $ANSIBLE_VAULT;1.1;AES256;prod          37636561366636643464376336303466613062633537323632306566653533383833366462366662          6565353063303065303831323539656138653863353230620a653638643639333133306331336365          62373737623337616130386137373461306535383538373162316263386165376131623631323434          3866363862363335620a376466656164383032633338306162326639643635663936623939666238          3161
```

What if you want to include multiple vaults in a single playbook? You can easily pass in multiple `vault-id` declarations on an `ansible-playbook` command line.

```
ansible-playbook --vault-id dev@/path/dev-vault-password-file --vault-id prod@/path/prod-vault-password-file site.yml
```

### Rekeying

Finally, it's important to regularly cycle your passwords. For files that are encrypted, you can use the command line below. Passing in the `new-vault-id` parameter makes it easy to change the password that the secrets are encrypted with.

```
ansible-vault rekey --vault-id prod@/path/prod-vault-password-file-old --new-vault-id prod@/path/prod-vault-password-file-new site.yml
```

As noted above, command line rekeying does not work for encrypted variables. In this case, you will need to individually re-encrypt the strings and replace them in a given playbook.

## Best Practices

Security is difficult, especially when it comes to using secrets within automation systems. With that in mind, below are several best practices to use when utilizing Ansible Vault. Though we have covered some of these previously, it is prudent to reiterate those practices.

- **ACL protected and unversioned password files**Password files mustn't be stored within version control systems, such as GIT. Additionally, make sure that only the appropriate users can access the password file.
- **Separate vaults**Normally, many different environments are in use. Therefore, it is best to separate the required credentials into the appropriate vaults.
- **Regular file and variable password rekeying**In the case of password reuse or leaks, it is best to regularly rekey the passwords in use to limit exposure.

## Conclusion

As with any automation system, it is critically important that secrets are properly protected and controlled. With Ansible Vault, that process is made easy and flexible. Using the best practices outlined above, storing and using secrets within Ansible is safe and secure.

To extend Ansible Vault even further and take this process to the next level, you can use scripts that integrate into password management solutions. As you can see, Ansible Vault is an excellent way to use secretes within Ansible playbooks.