"Predicting customer feelings before they buy a product"

If you try executing the code along with the YouTuber, then at some point in time, you will get an error that "Daemon functionality is not supported on Windows". Therefore, to resolve this issue, you need to install WSL (Windows subsystem for Linux) and Ubuntu, on your Windows laptop/computer. To install WSL and Ubuntu, just run your command prompt as Administrator and run the below command:

wsl --install

This will take some time to install and then you will be asked to restart your computer/laptop. After the restart, open your Ubuntu terminal (remember to open the Ubuntu terminal and not WSL terminal), and give your first name in small letter case, and then give an easy password (Note: while entering, the password will not be visible). After that, a main directory will be shown.

Now, create a folder in Local disk D as "customer_satisfaction" and paste that path in the Ubuntu terminal.

For example, the path should be like this: cd /mnt/d/customer_satisfaction

Now, copy or download all the files and folders from my repository and paste it in the Local D > customer_satisfaction folder. Also, for the dataset, you can either download it from the YouTuber's GitHub or directly download it from my Google Drive link present in the data folder. (I can't upload the 50Mb dataset as the maximum dataset can be uploaded up to 25Mb). Also, remember that, to use zenml, your Python version should be greater than 3.7 and less than 3.10. Therefore, in the below commands, we will run and install everything required for zenml, mlflow and streamlit and also, install and use the Python 3.9 version.

Once you are in the customer_satisfaction directory (cd /mnt/d/customer_satisfaction), run the below Ubuntu commands, one by one.

sudo apt update

apt list --upgradable

sudo apt update

sudo apt install wget software-properties-common

sudo add-apt-repository ppa:deadsnakes/ppa

sudo apt update

sudo apt install python3.9

python3.9 --version

sudo apt install python3.9-distutils

wget https://bootstrap.pypa.io/get-pip.py

sudo python3.9 get-pip.py

python3.9 --version

pip install zenml

pip3 install zenml["server"]

pip uninstall zenml

pip install zenml["server"]

Now, when you run the above command, it will give you an error as below.

Installing collected packages: zenml WARNING: The script zenml is installed in '/home/abdur/.local/bin' which is not on PATH. Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. Successfully installed zenml-0.52.0

Therefore, change the below path (here, I got it as '/home/abdur/.local/bin') and run the below command.

export PATH="$PATH:/home/abdur/.local/bin"

pip install zenml["server"] --no-warn-script-location

Now, from here the project starts. Run the below commands accordingly and see the outputs.

zenml init

Now, you also need to change the data path in the run_pipeline.py and run_deployment.py files. So, change it as shown below:

Eg: run_pipeline.py

from pipelines.train_pipeline import train_pipeline

if name == "main": train_pipeline(data_path="/mnt/d/customer_satisfaction/data/olist_customers_dataset.csv")

Now, run the below commands.

python3.9 run_pipeline.py

zenml up

zenml integration install mlflow -y

zenml stack list

zenml stack describe

zenml experiment-tracker register mlflow_tracker --flavor=mlflow

zenml model-deployer register mlflow --flavor=mlflow

zenml stack register mlflow_stack -a default -o default -d mlflow -e mlflow_tracker --set

zenml stack describe

python3.9 run_pipeline.py

Now, when you run the above command, you will immediately get a file path below it. So, copy that, paste it into the below command, and run it.

mlflow ui --backend-store-uri "file path obtained from python3.9 run_pipeline.py command"

( Eg: mlflow ui --backend-store-uri "file:C:\Users\abdur\AppData\Roaming\zenml\local_stores\c8cf9462-bc11-48dc-bece-2233ec4d3e76\mlruns" )

Now, run the below commands:

python3.9 run_deployment.py --config deploy

python3.9 run_deployment.py --config predict

pip install streamlit

streamlit run streamlit_app.py

Also, you can also visualize zenml and mlflow dashboards by running "zenml up" and the mlflow link obtained when you run the commands.

That's all. Hope it works for you.

For any help, contact me at my email: karthikkckarthik02@gmail.com or abdurahman.5july@gmail.com 

Thank you
