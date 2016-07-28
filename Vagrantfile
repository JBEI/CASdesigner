# -*- mode: ruby -*-
# vi: set ft=ruby :

SOURCE_FOLDER =  "/var/www/casdesigner"

Vagrant.configure(2) do |config|
  config.vm.box = "debian/jessie64"
  config.vm.network "forwarded_port", guest: 8888, host: 8888
  config.vm.synced_folder ".", SOURCE_FOLDER

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get install -y python-pip 
    sudo apt-get install -y python-dev # for Pandas
    sudo apt-get install -y pkg-config libfreetype6-dev libpng12-dev # for matplotlib
    sudo pip install jupyter biopython intermine pandas matplotlib xlrd

    # Configure Jupyter
    su - vagrant -c "jupyter notebook --generate-config"
    cat > /home/vagrant/.jupyter/jupyter_notebook_config.py <<-EOF
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
EOF
  SHELL

  config.vm.provision "shell", run: "always", inline: <<-SHELL
    su - vagrant -c "cd #{SOURCE_FOLDER} && jupyter notebook"
  SHELL
end
