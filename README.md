# Home Assistant LDAP Auth Script

This Python script performs a Home Assistant user authentication against an LDAP server. The script uses the `ldap3` library to connect to the LDAP server, search for a user, and authenticate the user based on the provided username and password.

## Installation

1. Save the script to your prefered place (e.g. `/config/auth.py`)

2. Make the script runnable by the HASS user (e.g. via SSH plugin)

   ```bash
   cd /PATH_TO_AUTH_PY/auth.py
   chmod +x auth.py
   ```

3. Open the Docker container of your HASS instance (e.g. via SSH plugin)

   ```bash
   docker exec -it homeassistant bash
   ```

4. Install the required pip module (e.g. via SSH plugin)

   ```bash
   pip install -t . ldap3
   ```

5. Add the following to your `configuration.yaml` file

   ```bash
   homeassistant:
       auth_providers:
           - type: command_line
             name: 'LDAP'
             command: '/usr/local/bin/python3'
             args: ['/PATH_TO_AUTH_PY/auth.py']
             meta: true # Ensure this is true - otherwise you won't able to set a username/group for new created users
           - type: homeassistant
   ```

6. Restart your HASS instance (**not just the configuration**)

7. You may now log in via LDAP auth

## LDAP Configuration

Update the following LDAP configuration variables in the script according to your LDAP server settings:

- `SERVER`: LDAP server URL (e.g., "ldap://YOUR_SERVER_IP:389").
- `HELPERDN`: Helper LDAP user DN (e.g., "cn=LDAPSEARCH_USERNAME,ou=users,dc=ldap,dc=goauthentik,dc=io").
- `HELPERPASS`: Helper LDAP user password.
- `BASEDN`: Base DN for LDAP search (e.g., "dc=ldap,dc=goauthentik,dc=io").
- `ATTRS`: Attributes to retrieve during LDAP search (e.g., "cn").
- `BASE_FILTER`: Base filter for LDAP search (e.g., "(objectClass=person)").

## Error Handling

The script checks for the existence of required environment variables and handles LDAP connection errors. If the script fails, it prints an error message to standard error and exits with a non-zero status code.

## Note

**Authentik:** If you use Authentik LDAP service you have to make sure that you enable case sensitivity in your LDAP flow! Otherwise HASS will create a user for the uppercase user (e.g. Test) and the lowercase user (e.g. test).

Ensure that the LDAP server is accessible from the machine running the script and that the provided LDAP user credentials are valid.

Feel free to customize the script based on your specific LDAP server configuration and authentication requirements.

## Documentation

- [Home Assistant Authentication Providers](https://www.home-assistant.io/docs/authentication/providers/) check out the HASS Docs to learn more about how to use LDAP.
- [Authentik LDAP Provider](https://goauthentik.io/docs/providers/ldap/) learn more about using Authentik as LDAP server.

## Credits

Special thanks to [yumenohikari](https://gist.github.com/yumenohikari) for the original inspiration from the [ha-ldap-auth](https://gist.github.com/yumenohikari/8440144023cf33ab3ef0d68084a1b42f) gist.

## License

This repository is licensed under the [GNU General Public License v3.0](LICENSE). Feel free to use, modify, and distribute it. If you encounter any issues or have suggestions, please create an issue in the [GitHub repository](https://github.com/panteLx/HASS-LDAP-Auth).
