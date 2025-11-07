Add user email and name

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Generate a new SSH key (RSA 4096-bit):

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add your key to the agent:

```bash
ssh-add ~/.ssh/id_rsa
```

Display your public key, then copy it to your clipboard:

```bash
cat ~/.ssh/id_rsa.pub
```

Finally, test your SSH connection with GitHub:

```bash
ssh -T git@github.com
```