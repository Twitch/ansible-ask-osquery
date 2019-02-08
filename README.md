# wat iz
Ansible to run osqueri queries on hosts, collect the output, and store it locally on your Ansible host. These JSON files can then be processed by Logsash and stored in Elasticsearch.

It's no Fleet, but it will answer questions for now and can be scheduled.

# How To
## Running
Once checked out, **you must create your own user-vars.yml.** This will define your username and the path to your private key for the environments. You can crib from the user-vars.template to create your own.

From the top level directory you can run:
`$ ansible-playbook plays/osq.questions.yml --ask-sudo-pass -i inventories/($inv.file.yml)`

You will be prompted for your SUDO password to the environment (leave blank if not required) and the group name you'd like to target.

## Output
This play produces files with output in JSON formatting. Each "question" from each remote host will have its own file. Output files are organized like so:

./output/{location_name}/{osquery_table}/{hostname}.{table}.out

This makes it easier to identify files on disk and during logstash processing.

# Things that aren't great
Check out issues in the repo, I'll keep those more update than the README, likely.

## Optimization 
### Time to complete
It takes way too long to run. Currently about 130m for ~500 hosts. I don't know if there's a way to speed this up or optimize it, but it sure would be nice if it wasn't a ~3hr task to poll the datacenters.

### Code re-use
This is my first ansible, but it feels like there *should* be a way to write an array or dictionary of the osquery tables we'd like to pull and build the tasks dynamically instead of having so much static code.

