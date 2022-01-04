# k8s-cluster-scenarios
Contains the yaml files to be used for different scenario in kubernetes cluster.
NOTE : Make sure to create secret with name "regcred" in the default namespace. To pull images at runtime from docker hub the ullsecret would be used.
server-config.yaml - Yaml configuration file capturing the details of the k8s cluster. Both control & data plane information must be provided in the file.
        cluster:
        - 
            controlplanes: # --> All control plane/master nodes IP Address must be listed under these.
            workers: # --> All data plane/worker nodes IP Address must be listed under these.
            
user-config.yaml - This file contains the configuration specification of ssh port,username & password to connect to the nodes in the cluster. Please make sure that all WNs can be connected using the same username & password. Also make sure that the sudo for the user is configured with not password input.
test-case-files.yaml - This file contains the list of the test cases that are selected/considered for execution. However if the test case is mentioned in the 'ignore-tc.yaml' then it would be ignored and not selected for execution.
ignore-tc.yaml - This file contains the list of test cases that must not be considered for execution.
disrupt-cluster.py - Actual script to create disruption in the configured cluster. Thsi can be launched using 'py disrupt-cluster.py'
disrupt-cluster.log - Log file to track or debug any issues for the script execution.


#How add to add new testcase to this list.
Create File TC<NUM> in the current directory with following specification:
    plane: "control" # Mention where to execute the command. For commands with kubectl you can keep control or which ever node has the access. Also if you select "data" here then it would execute the command on randomly selected WNs from the list in server-config.yaml.
    execute-command-on: "1" #execute command on how many machines(data,control plane)
    command: # List of commands that are to be executed on the selected nodes.
    - "kubectl get pods -n default"
    exepcted-effect: "Description to be printed on the console goes here" # The message on the console to be printed on the console after the script is run. Should give the message to the user to handle where to look for the problem. In case some files needs to be used then they can be created in same directory and pushed to github. They can be downloaded using commands and executed.
    fix:
    - "kubectl get pods -n default" #Here the list of commands should be mentioned that reverts the effects of the command executed for creating the problem.
    wait-to-fix: 5 #The duration in MINUTES for to wait for the fix or automatically execute the commands under "fix".