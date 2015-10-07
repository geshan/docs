# Using git Submodules

You will have to fetch submodules manually. Below a snippet of a `wercker.yml` step
that does exactly that:

```yaml
   - script:
        name: install git
        code: |
            apt-get update
            apt-get install git -y
    - add-ssh-key:
        keyname: KEY_NAME
        host: github.com
    - add-to-known_hosts:
        hostname: github.com
        fingerprint: 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48
    - script:
        name: initialize git submodules
        code: |
            git submodule update --init --recursive
```

Note: fingeprint values sometimes change, for the most recent information see: [values for
github](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)
and [values for bitbucket](https://blog.bitbucket.org/?s=fingerprint)
