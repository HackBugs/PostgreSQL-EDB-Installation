> # Sometime unable to vi file getting error
> ### This error indicates that a swap file (.edb-pgbouncer-1.23.ini.swp) was left behind, possibly due to a previous crash or an unclean exit while editing the edb-pgbouncer-1.23.ini file. Here's how to resolve it:

# Step 1: Check if another process is editing the file
lsof /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini

# If the file is being edited by another process, you'll see details about it.
# In that case, either wait for the process to finish or terminate it:
kill -9 <process_id>

# Step 2: Recover the swap file (if you want to retain changes from the previous session)
vim -r /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini

# Follow the prompts in Vim to recover the file. Save and exit with:
:wq

# Step 3: If you don't need to recover, delete the swap file
sudo rm /etc/edb/pgbouncer1.23/.edb-pgbouncer-1.23.ini.swp

# Step 4: Re-edit the original file
sudo vim /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini
