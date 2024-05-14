### These are the steps for using SSH keys for accessing and using GitHub on remote servers
As of 5/14/24, you can follow the steps in this documentation:  
https://docs.github.com/en/enterprise-cloud@latest/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

### I already had the public/private SSH key pair in my .ssh directory on my local computer, so I skipped the "Generating new SSH key" step

If you're going to use the same keys for different computers (i.e. I've set up two different laptops), the same key pair files (id_25519 and id_25519.pub) have to be in the `.ssh` directory of each computer.

### Create the config file

If it doesn't exist, create a file at `~/,ssh/config`

For the SSH keys to work with GitHub, make sure this is in the config file:
```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

You can remove the "UseKeychain" line if you don't want to have to type in a password

### Add the private SSH key to the SSH Agent
`ssh-add ~/.ssh/id_ed25519`

### Test that the local key works with GitHub
`ssh -T git@github.com`

That should return a message along the lines of "Hi USERNAME! You've successfully authenticated, but git does not provide shell access"

This step now confirms that the key can be read by GitHub without needing to type in a password. The next step is to get this to work from a remote server as well.

### Set up SSH agent forwarding
From the local computer, edit the `~/.ssh/config` file by adding new lines:
```
Host [IP_address_for_server_I_want_github_to_work_with]
	ForwardAgent yes
```

Now you should be able to log in to the server, without entering a password (since it is reading the SSH key as the password), and you should be able to use github from that server using that same SSH key.

To test it, log in to the server, and then type:
`ssh -T git@github.com`
