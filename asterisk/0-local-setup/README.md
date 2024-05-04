# Local setup
This section will cover the following:
- Local setup of asterisk inside docker container
- Usage of a SIP phone that will connect to the Asterisk server
- Simple configuration necessary for the SIP phone to register itself inside asterisk


## Setup explanation

### Runtime
We chose docker as our runtime engine and we have created `docker-compose.yaml` file where we will define the configuration of the asterisk.

The docker compose file uses [andrius/asterisk](https://hub.docker.com/r/andrius/asterisk/#!) docker image.

Configuration is mounted as a volume. The volume is rewriting the asterisk configuration stored in `/etc/asterisk/`. If you would like to play with the configuration change files in `config` folder.

### Configuration
Configuring asterisk is a complex task. To make this playground simple we will start by minimal configuration where the goal will be to connect our SIP phone to the asterisk.

> Note: We are rewriting the configuration of asterisk, therefore the following files we mention will be the only one used by asterisk.

#### modules.conf

We will copy the default modules file from `../default-files/asterisk/etc/modules.conf` to `config/modules.conf`.
Without the the `modules.conf` file asterisk doesn't start properly:

```log
[May  4 08:53:58]  Asterisk Dynamic Loader Starting:
[May  4 08:53:58] WARNING[1]: config.c:2041 process_text_line: parse error: No category context for line 1 of /etc/asterisk/modules.conf
[May  4 08:53:58] WARNING[1]: loader.c:2240 loader_config_init: 'modules.conf' invalid or missing.
[May  4 08:53:58] ERROR[1]: asterisk.c:4039 check_init: Module initialization failed.  ASTERISK EXITING!
```
