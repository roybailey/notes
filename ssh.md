SSH
--

##### Create a public/private key for remote system (e.g. GitHub/BitBucket)

Location of public/private keys

`cd ~/.ssh`

Create new RSA key (assigning name to differentiate from other keys, passphrase optional)

`ssh-keygen -t rsa -C "my@email.com"`

Add private key to local ssh agent

`ssh-add ~/.ssh/<name>_rsa`

Copy public key into clipboard for registration on remote system

`cat ~/.ssh/<id_rsa>.pub | pbcopy`

