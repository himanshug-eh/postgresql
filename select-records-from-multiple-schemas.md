# How to SELECT records from a TABLE from multiple SCHEMAS

We can write a anonymous psql procedure to get this done and following is an example to do so:

```
DO $$DECLARE 
    t_name RECORD;
    r_email TEXT;
BEGIN
    FOR t_name IN SELECT DISTINCT schemaname FROM pg_catalog.pg_tables WHERE schemaname NOT IN ('public', 'information_schema', 'pg_catalog')
    LOOP
        FOR r_email IN EXECUTE 'SELECT DISTINCT email FROM "'||t_name.schemaname||'".users'
        LOOP
            RAISE NOTICE '%', r_email;
        END LOOP;
    END LOOP;
END$$;
```

And here is an example to run the psql procedure from command line:

```
sudo -u postgres psql -t saas -c "DO \$\$ DECLARE t_name RECORD; r_email TEXT; BEGIN FOR t_name IN SELECT DISTINCT schemaname FROM pg_catalog.pg_tables WHERE schemaname NOT IN ('public', 'information_schema', 'pg_catalog') LOOP FOR r_email IN EXECUTE 'SELECT DISTINCT email FROM \"'||t_name.schemaname||'\".users' LOOP RAISE NOTICE '%', r_email; END LOOP; END LOOP; END\$\$;"
```
