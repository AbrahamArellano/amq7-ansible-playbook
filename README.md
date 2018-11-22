- Run installation:

sudo ansible-playbook -i hosts.yml amq-install.yaml

- Run uninstall:

sudo ansible-playbook -i hosts.yml amq-uninstall.yaml


- Create queue:

	$AMQ_MASTER_HOME/bin/artemis queue create --auto-create-address --address dzbank.queue.test --name dzbank.queue.test --preserve-on-no-consumers --durable --anycast --user amq --password amq --url tcp://127.0.0.1:61616

- Producer:

	+ Master:

		$AMQ_MASTER_HOME/bin/artemis producer --message-count 10 --url tcp://127.0.0.1:61616 --destination queue://dzbank.queue.test --user amq --password amq
		
	+ Slave:
		
		$AMQ_SLAVE_HOME/bin/artemis producer --message-count 10 --url tcp://127.0.0.1:62616 --destination queue://dzbank.queue.test --user amq --password amq
		
- Consumer:

	+ Master:
	
		$AMQ_MASTER_HOME/bin/artemis consumer --message-count 100 --url tcp://127.0.0.1:61616 --destination queue://dzbank.queue.test --sleep 1000 --verbose --user amq --password amq
	
	+ Slave:
	
		$AMQ_SLAVE_HOME/bin/artemis consumer --message-count 100 --url tcp://127.0.0.1:62616 --destination queue://dzbank.queue.test --sleep 1000 --verbose --user amq --password amq
		
		
- Test:

	./artemis queue stat --queueName dzbank.queue.test --user amq --password amq
