# setup-ssh-client

GitHub Action for setting up everything needed for connecting to a remote SSH server from the runner.

> [!WARNING]
>
> This action overwrites any existing SSH keys already saved in the `~/.ssh/id_rsa` file.

## Usage

```yaml
jobs:

  do-the-thing:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Setup SSH client
        uses: tichopad/setup-ssh-client@v1
        with:
          ssh-key: ${{ secrets.SSH_KEY }}

      - name: Connect to SSH server
        run:  ssh -T -o StrictHostKeyChecking=no user@example.com 'echo "Hello, world!"'
```

If you want to avoid using the `StrictHostKeyChecking=no` option for better security, you can add the host key to the known hosts file by using the `known-hosts` input:

```yaml
jobs:

  do-the-thing:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Setup SSH client
        uses: tichopad/setup-ssh-client@v1
        with:
          ssh-key: ${{ secrets.SSH_KEY }}
          known-hosts: ${{ secrets.KNOWN_HOSTS }}

      - name: Connect to SSH server
        run:  ssh -T user@example.com 'echo "Hello, world!"'
```