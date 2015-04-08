# Notice of Export Control Law

This software distribution includes cryptographic software that is subject to the U.S. Export Administration Regulations (the "*EAR*") and other U.S. and foreign laws and may not be exported, re-exported or transferred (a) to any country listed in Country Group E:1 in Supplement No. 1 to part 740 of the EAR (currently, Cuba, Iran, North Korea, Sudan & Syria); (b) to any prohibited destination or to any end user who has been prohibited from participating in U.S. export transactions by any federal agency of the U.S. government; or (c) for use in connection with the design, development or production of nuclear, chemical or biological weapons, or rocket systems, space launch vehicles, or sounding rockets, or unmanned air vehicle systems.You may not download this software or technical information if you are located in one of these countries or otherwise subject to these restrictions. You may not provide this software or technical information to individuals or entities located in one of these countries or otherwise subject to these restrictions. You are also responsible for compliance with foreign law requirements applicable to the import, export and use of this software and technical information.

# Configuration

This plugin provides a DNS integration for OpenShift Enterprise version 2.2.
The plugin essentially allows calling of a local script that can be used to integrate with a remote system as required.  In the example the script calls local nsupdate commands.

The configuration file for the plugin is ```/etc/openshift/plugins.d/openshift-origin-dns-custom.conf```

Three variables in the configuration file define the location of the update server:
    # The DNS server
    DNS_CUSTOM_SCRIPT_NAME="/usr/local/bin/ose-dns-custom"
    
# Build - generates the gem file
gem build openshift-origin-dns-custom.gemspec

# Manually Install:
gem install -V --local --install-dir /opt/rh/ruby193/root/usr/share/gems --force ./openshift-origin-dns-custom-1.0.0.gem

restorecon -Rv /opt

cp /opt/rh/ruby193/root/usr/share/gems/gems/openshift-origin-dns-custom-1.0.0/conf/openshift-origin-dns-custom.conf.example /etc/openshift/plugins.d/
cp /opt/rh/ruby193/root/usr/share/gems/gems/openshift-origin-dns-custom-1.0.0/conf/ose-dns-custom /usr/local/bin/
mv /etc/openshift/plugins.d/openshift-origin-dns-nsupdate.conf /etc/openshift/plugins.d/openshift-origin-dns-nsupdate.conf.save
cp /etc/openshift/plugins.d/openshift-origin-dns-custom.conf.example /etc/openshift/plugins.d/openshift-origin-dns-custom.conf

cp /var/named/mydomain.key /etc/openshift/
chown apache:root /etc/openshift/mydomain.key

edit /usr/local/bin/ose-dns-custom as required, for local DNS server update the domainname and keyfile name, for custom DNS change add and delete code as required
     Note: if using on second broker host, remote to the DNS server, need to remove the -l from nsupdate and insert 'server <IP address>' entry.

Restart openshift-* services, broker first then console afterwards.

# Note: oo-accept-broker NOTICE
The oo-accept-broker script has a descrete list of dynamic DNS plugins in a case statement so there is a warning NOTICE raised when the broker is checked, this is just a warning from the check script and not an issue with the OpenShift::CustomDNSPlugin class not being known.

