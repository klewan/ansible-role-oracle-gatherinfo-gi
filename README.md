Ansible Role: oracle-gatherinfo-gi
==================================

This role automatically discovers Oracle Grid Home and gathers Grid Infrastructure data, such as GRID_HOME, installed software version, list of RAC nodes, applied patches, ASM disks and disk groups.

Collected information is stored in role's `oracle_gatherinfo_gi_gi_info` variable and global `oracle_gi_info` variable.

Sample output:

    "oracle_gi_info": {
        "oracle_home": "/u01/app/12.1.0/grid",
        "oracle_home_patches": [
            {
                "patch_description": "ACFS PATCH SET UPDATE 12.1.0.2.181016 (28259950)",
                "patch_number": "28259950"
            },
            {
                "patch_description": "OCW PATCH SET UPDATE 12.1.0.2.181016 (28259914)",
                "patch_number": "28259914"
            },
            {
                "patch_description": "Database Patch Set Update : 12.1.0.2.181016 (28259833)",
                "patch_number": "28259833"
            },
            {
                "patch_description": "WLM Patch Set Update: 12.1.0.2.180116 (26983807)",
                "patch_number": "26983807"
            },
            {
                "patch_description": "Database PSU 12.1.0.2.3, Oracle JavaVM Component (Apr2015)",
                "patch_number": "20415564"
            }
        ],
        "rac_nodes": [],
        "rac_remote_nodes": [],
        "software_version": "12.1.0.2.0"
    }


Supported OS:
-------------
* RedHat
* CentOS
* OracleLinux

Requirements
------------

This role uses `oracle` role.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    # Dictionary describing installed Grid Infrastructure (dynamically populated)
    oracle_gatherinfo_gi_gi_info: {}

    # e.g.:
    #oracle_gatherinfo_gi_gi_info:
    #  oracle_home: "/u01/app/12.1.0/grid"
    #  rac_nodes: []
    #  rac_remote_nodes: []
    #  software_version: "12.1.0.2.0"

    # print collected GI information (display oracle_gatherinfo_gi_grid_home variable)
    oracle_gatherinfo_gi_print_collected_data: true

    # get GRID_HOME version
    oracle_gatherinfo_gi_get_grid_home_version: true

    # get GRID_HOME patches
    oracle_gatherinfo_gi_get_grid_home_patches: false

    # get GRID_HOME RAC node names
    oracle_gatherinfo_gi_get_rac_nodes: true
    oracle_gatherinfo_gi_get_local_rac_node: true

    # get extra facts (for example for capacity report)
    oracle_gatherinfo_gi_get_extra_facts: false

    # facts directory
    oracle_gatherinfo_gi_facts_dir: "{{ oracle_facts_dir | default('~/scripts/facts.d', true) }}"

    # a list of fact scripts templates used by Ansible 'setup' module
    oracle_gatherinfo_gi_fact_script_templates:
      - asm_diskgroups.fact
      - asm_disks.fact
      - grid_home.fact


Dependencies
------------

This role uses `oracle` role.

Example Playbook
----------------

    - name: Gather information about installed Oracle components
      hosts: ora-servers
      gather_facts: true
      become: true
      become_user: '{{ oracle_user }}'

      tasks:

      - import_role:
          name: oracle-gatherinfo-gi
        tags:
          - oracle-gatherinfo-gi
          - oracle-gatherinfo-allcomponents
		  

Inside `vars/main.yml` or `group_vars/..` or `host_vars/..`:

    
    #------------------------------------------------
    # overrides role 'oracle-gatherinfo-gi' variables
    #------------------------------------------------

    # get GRID_HOME patches
    oracle_gatherinfo_gi_get_grid_home_patches: true

    # get extra facts (for example for capacity report)
    oracle_gatherinfo_gi_get_extra_facts: true
	
    # ... etc ...


License
-------

GPLv3 - GNU General Public License v3.0

Author Information
------------------

This role was created in 2018 by [Krzysztof Lewandowski](mailto:Krzysztof.Lewandowski@fastmail.fm).


