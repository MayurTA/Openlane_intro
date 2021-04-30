# Introduction to Openlane
After you have installed Openlane on your system, you will have two new directories, one named '_openlane_' and the name of the other one is set by the you during installation, which in my case is '_pdks_'. '_openlane_' folder contains all the designs, previous runs, etc. and this is where we run the software. '_pdks_' folder contains the tech file, the pdk information etc. which will be used while running a design.

## Adding your design into Openlane
The first stage in Openlane flow is synthesis, during which conversion of RTL into standard cells happen. So, if you need to add your design into the flow, you need to add the verilog code of your design into Openlane.

To add a new design, navigate to the folder `openlane/designs`. In the designs folder you will the folders of various other inbuilt designs. Now, create a new folder here for your design. __Make sure that the name of the folder is same as the name of the main module in your verilog code__. Now inside that folder, create two new folders with names '_src_' and '_runs_'. Place your verilog code inside the '_src_' folder. And go into some other design folder, say inverter, and copy the five .tcl files whose name starts from sky130A.. and paste them into your new design folder. So, to summarize, the file heirarchy pertaining to our design at this point would be, 
```
openlane
  |--> designs
        |--> my_traffic
              |--> runs
              |--> src
                    |--> traffic.v
              |--> sky130A_sky130_fd_sc_hd_config.tcl
              |--> sky130A_sky130_fd_sc_hdll_config.tcl
              |--> sky130A_sky130_fd_sc_hs_config.tcl
              |--> sky130A_sky130_fd_sc_ls_config.tcl
              |--> sky130A_sky130_fd_sc_ms_config.tcl
```
  where, my_traffic is my design name and traffic.v is my verilog code of the design.
  
  Now, you've added your design in the format required by Openlane to process it. Next, we need to run the design.
  
  ## Running your design in Openlane
  Before starting openlane, we first need to get the docker running. Go back into the '_openlane_' folder and open the terminal. And run the following commands,
  ```
  export PDK_ROOT=absolute_path_to_pdk_folder                                     (Example : export PDK_ROOT=/home/mayur/pdks)
  
  docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc6
  ```
  Now the docker is running and we can open Openlane. Openlane can be run in two modes,
   - __Non-interactive mode__ - In this mode, you just run one command and all the stages are automatically performed.
   - __Interactive mode__ - In this mode, you need to manually run each stage, hence gives you more freedom to analyse and change different environment variables for a better output.
  
  Let's look at automated mode. 
  Before we run our design inside openlane, we need another file named '_cofig.tcl_' inside our design folder which gives an overview information about our design to the openlane. So, we initially run openlane with a tag `init_design_config`. So, run the following command further in the terminal, after we have run the docker.
  ```
  ./flow.tcl -design <design_name> -init_design_config                                 (Example : ./flow.tcl -design my_traffic -init_design_config)
  ```
  This command creates a '_config.tcl_' file inside our design folder, which you can verify.
  So the file hierarchy now is, 
  ```
  openlane
  |--> designs
        |--> my_traffic
              |--> runs
              |--> src
                    |--> traffic.v
              |--> sky130A_sky130_fd_sc_hd_config.tcl
              |--> sky130A_sky130_fd_sc_hdll_config.tcl
              |--> sky130A_sky130_fd_sc_hs_config.tcl
              |--> sky130A_sky130_fd_sc_ls_config.tcl
              |--> sky130A_sky130_fd_sc_ms_config.tcl
              |--> config.tcl
```
Now, we're good to go. To run your design in openlane, type the folowing command.
```
./flow.tcl -design <design_name>
```
This command runs all the stages and creates corresponding outputs in the folder `my_traffic/runs`.
  
