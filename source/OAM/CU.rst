openstack image create --disk-format=qcow2 --container-format=bare --file=AirFrameVirtualized-4.3002.6-CHN-ART01.qcow2 ft4.3002.6

heat stack-delete ECE_CI_Stack_789
ECE_CI_ext_nets_789

heat stack-create -f 5gbts_heat.yaml -e 5gbts_heat.env  ECE_CI_Stack_789



cde0d20b-f9f3-40f8-857c-011187fba840
