# EC2 SSH Access Recovery Guide

If you've lost your PEM key file and need to regain SSH access to your EC2 instance, follow these steps while logged in to your instance via EC2 Connect or another method:

## Steps to Create a New SSH Key

1. Generate a new SSH key pair:
   ```bash
   ssh-keygen -t rsa
   ```
   This will create `id_rsa` (private key) and `id_rsa.pub` (public key) files in the `/root/.ssh/` directory.

2. Navigate to the `.ssh` directory:
   ```bash
   cd /root/.ssh/
   ```

3. Convert the private key to PEM format:
   ```bash
   ssh-keygen -p -m PEM -f id_rsa
   ```
   This converts the private key to PEM format which is required for SSH clients.

4. Copy the public key to the authorized_keys file:
   ```bash
   cp id_rsa.pub authorized_keys
   ```
   This allows connections using the newly created private key.

5. Display the private key content:
   ```bash
   cat id_rsa
   ```
   Copy the entire output (including BEGIN and END lines) to create your new PEM file.

## Using Your New Key

1. Copy the entire private key content (displayed in step 5) to a new file on your local machine.
2. Save the file with a `.pem` extension (e.g., `new-instance-key.pem`).
3. Set the correct permissions for the key file:
   ```bash
   chmod 400 new-instance-key.pem
   ```
4. Connect to your instance using the new key:
   ```bash
   ssh -i new-instance-key.pem root@your-instance-ip
   ```

```

For downloading files from the instance:
```bash
scp -i new-instance-key.pem root@your-instance-ip:/remote/path/file.txt /local/path/
```
