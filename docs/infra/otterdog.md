# Otterdog

Our organization configs are managed on [.eclipsefdn](https://github.com/eclipse-pass/.eclipsefdn)
using the [Otterdog tool](https://gitlab.eclipse.org/eclipsefdn/security/otterdog)

Our first project will be to manage secrets via [bitwarden](/docs/infra/bitwarden.md).

## Configurations

The base configuration is [eclipse-pass.jsonnet](https://github.com/eclipse-pass/.eclipsefdn/blob/main/otterdog/eclipse-pass.jsonnet),
and can be monitored at [eclipse-pass.org/.eclipsefdn](https://eclipse-pass.org/.eclipsefdn/)

![Otterdog Config Monitoring](/docs/assets/otterdog/dashboard.png)

### Organizational Secrets

Here is documentation on managing [organization secrets](https://otterdog.readthedocs.io/en/latest/reference/organization/secret/)

```jsonnet
orgs.newOrg('eclipse-pass') {
  secrets+: [
    orgs.newOrgSecret('HELLO_WORLD_QUEST') {
      value: "bitwarden:23801ca4-fd27-446c-b5af-b07b0108f443@quest",
      visibility: "public",
    },
    orgs.newOrgSecret('HELLO_WORLD_COLOR') {
      value: "bitwarden:23801ca4-fd27-446c-b5af-b07b0108f443@color",
      visibility: "public",
    },
  ],
}
```

Please refer to [bitwarden for specifics on password management](/docs/infra/bitwarden.md)

## Playground (Online Editor)

From the [monitoring application]((https://eclipse-pass.org/.eclipsefdn/)) you can access a [otterdog playground](http://eclipse-pass.org/.eclipsefdn/playground/)

![Otterdog Playground](/docs/assets/otterdog/playground.png)

## Installing Locally

If you are testing github integrations, you will need to [install Otterdog locally](https://otterdog.readthedocs.io/en/latest/install/)

Below is a summary of the [installation guide above](https://otterdog.readthedocs.io/en/latest/install/).
If you run into problems, then please read the source installation guide and update these docs.

You will need the following tools

```bash
python3 --version # 3.10+, tested on 3.11.4
pip --version     # any,   tested on 23.2
poetry --version  # any,   tested on 1.4.2
go version        # 1.13+, tested on 1.20.5
jb --version      # any,   tested on 'dev' (aka 0.5.1 as shown in troubleshooting below)
bw -v             # any,   tested on 2023.9.0
```

Now you can clone the [Otterdog repo](https://gitlab.eclipse.org/eclipsefdn/security/otterdog/)
and build it locally

```bash
# Somewhere outside of the eclipse-pass repos
git clone git@gitlab.eclipse.org:eclipsefdn/security/otterdog.git
cd otterdog
make init
```

You now have `otterdog` installed locally.

```bash
otterdog --version
# otterdog.sh, version 0.3.0.dev0
```

### Troubleshooting

#### jb / jsonnet-bundler not found

Here's how you install [jsonnet-bundler](https://github.com/jsonnet-bundler/jsonnet-bundler) (aka `jb`)

```
go install -a github.com/jsonnet-bundler/jsonnet-bundler/cmd/jb@v0.5.1
```

Note that [jsonnet-bundler](https://github.com/jsonnet-bundler/jsonnet-bundler) was installed to a `$HOME/go/bin` which was NOT
on our path, so I had to add that to ensure `jb` was accessible.

```bash
# in your ~/.bash_profile or similar
PATH="$HOME/go/bin:$PATH"
```

#### bw / bitwarden not found

Here's how you install [bitwarden](https://github.com/bitwarden/clients) (aka `bw`).

The installation uses `snap`, but (home)`brew` also works

```bash
brew install bitwarden-cli
```

#### .local/bin/otterdog: No such file or directory

If you see something like

```bash
test -f ~/.local/bin/otterdog || ln -s /Users/aforward/sin/projects/eclipse-pass/otterdog/otterdog.sh ~/.local/bin/otterdog
ln: /Users/aforward/.local/bin/otterdog: No such file or directory
make: *** [init] Error 1
```

Then ensure that you have a `~/.local/bin` to receive the file, and that is also on your path

```bash
mkdir -p ~/.local/bin

# in your ~/.bash_profile or similar
PATH="$HOME/.local/bin:$PATH"
```
