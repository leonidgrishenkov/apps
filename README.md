Create new docker network:

```sh
docker network create frontend
```

```sh
sudo apt update \
    && sudo apt install -y apache2-utils argon2
```

# Traefik

```sh
htpasswd -nb yc-user password
```

Copy output and add it to the `./traefik/config/config.yaml`.

```sh
task sync
```

Up the container:

```sh
cd ./traefik

docker compose --file ./docker/compose.yaml up --detach
```

```sh
docker logs -f traefik
```

# Vaultwarden

```sh
mkdir -p ./vaultwarden/data
```

Users can be invited through admin panel, so we need to create admin token to access admin panel.

Export and hash token:

```sh
export VAULTWARDEN_ADMIN_TOKEN=

export VAULTWARDEN_ADMIN_TOKEN_HASHED=$(echo -n $VAULTWARDEN_ADMIN_TOKEN | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4)
```

```sh
cd ./vaulwarden
```

Check if compose file is correct:

```sh
docker compose --file ./docker/compose.yaml config
```

Up container:

```sh
docker compose --file ./docker/compose.yaml up --detach
```

```sh
docker logs -f vaultwarden
```

Admin panel will be accessed on: [Admin panel](https://vaultwarden.leonidgrishenkov.com/admin)

In panel go to users and invite your user. After that you can create new account by clicking Sing up button, dispite signup disabling.

[About security of admin tokens](https://github.com/dani-garcia/vaultwarden/wiki/Enabling-admin-page#secure-the-admin_token)
