---
# zabbix agent install

- name: "Install repo for zabbix"
  apt:
    deb: https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian11_all.deb
    state: present

- name: Install zabbix-agent.service debian
  apt:
    name:
    - zabbix-agent
    - zabbix-sender
    - zabbix-get
    update_cache: yes
  notify:
    - zabbix-agent.service enable

- name: "touch file zabbix_agent.conf"
  template:
    src: "zabbix_agentd.conf.j2"
    dest: "/etc/zabbix/zabbix_agentd.conf"
    owner: zabbix
    group: zabbix
    mode: 0644
  notify:
    - restart zabbix-agent.service
