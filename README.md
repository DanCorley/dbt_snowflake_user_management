This sample dbt repo is setup to house examples of snowflake object management with _dbt.
It contains the following macros, with example data in the [./snowflake](./snowflake/) folder.



### [manage_snowflake_users](./macros/manage_snowflake_users.sql)

This macro helps with the creation and management of snowflake users. Any new users that are in the users file will be created and existing will be updated. It is non-destructive and will only disable users with the `disabled` flag.

The variable `DRY_RUN` will default to True, which will log statements without sending to snowflake for execution.

In order to not version control / store passwords in plain text, you will also need to pass in the variable `PASSWORD` which can be passed to your end users, and changed upon their first login.

Add any new users and their attributes to `./snowflake/users/users.yml` and run:
```bash
dbt run-operation manage_snowflake_users \
  --args "$(cat ./snowflake/users/users.yml)" \
  --vars '{PASSWORD: $3cr3t}' # add {DRY_RUN: False} to execute
```
___
### [create_whitelist](./create_whitelist.sql)
  
This macro helps with management of whitelists and takes a multiple file approach - it will overwrite exising with the ip addresses included in the .yml. If adding new IPs, it is also best practivce to document the associated services/ users.

```bash
dbt run-operation create_whitelist \
  --args "$(cat ./snowflake/whitelist/{file_name}.yml)" \
  # add --vars {DRY_RUN: False} to execute
```

___

### _dbt resources:
- Learn more about dbt [in the docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](https://community.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
