Please visit below repo for updated version with ansible2 and without roles. This is simpler than the current one.
https://github.com/ajitsingh25/weblogic-patching


1) Global Variables:
filename: <playbook_dir>/group_vars/all

2) Specify application name i.e CS, GOLD, GEMS
Ex- app_name: CS

3) Specify JVMS to start/stop. For stage "ALL" option is default. For Live choose from below incase sensitive options:
ALL = All JVMs in a domain
NODE_ALL = ALL JVMs in a domain per machine
L1  = Lane 1 JVMS in a domain per machine
L2  = Lane 2 JVMS in a domain per machine

4) Specify sudo user & group.
Ex-
oracle_user: website
oracle_group: website

5) Specify Oracle Weblogic version to patch
oracle_version: 12130

6) Specify RollOut Patch Month
Ex- patch_name: APR2018

7) Specify RollBack Patch Month
Ex- patch_name_rollback: JAN2018

8) Specify below variable to True to kick off Patch Rollback else default should be False.
Ex- rollback_psu: True

#LIVE COMMANDS
1) Patch Live Env with debug enabled. To enable debug remove "--skip-tags debug" from below command. Below command will perform following functions:
  a) stop MS if running         (Role=ms_stop_role)
  b) stop NM if running         (Role=nm_stop_role)
  c) stop Admin if running      (Role=admin_stop_role)
  d) patch servers if running   (Role=patch)
  e) start NM if not running    (Role=nm_start_role)
  f) start Admin if not running (Role=admin_start_role)
  g) start MS if not running    (Role=ms_start_role)
  
COMMAND=ansible-playbook live_site.yml -i inventory/live/hosts --skip-tags debug

2) Run any of the specific function from above like stop MS only try below command.

COMMAND=ansible-playbook live_site.yml -i inventory/live/hosts --tags ms_stop_role --skip-tags debug

#STAGE COMMANDS (Similar to LIVE)
1) Patch Live Env with debug enabled. To enable debug remove "--skip-tags debug" from below command. 
  
COMMAND=ansible-playbook stage_site.yml -i inventory/stage/hosts --skip-tags debug

2) Run any of the specific function from above like stop MS only try below command.

COMMAND=ansible-playbook stage_site.yml -i inventory/stage/hosts --tags ms_stop_role --skip-tags debug
COMMAND=ansible-playbook stage_site.yml -i inventory/stage/hosts --tags ms_start_role --skip-tags debug




