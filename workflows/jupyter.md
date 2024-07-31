# Jupyter

Use Jupyter lab for:

1. Prototyping code (strongly recommend also having a .py file open to transfer your code to)
2. High level running of experiments
3. Visualizing data / results

**Beware that it is really easy to end up with a messy and convoluted notebook** if you do not regularly transfer code out of the notebook into a separate .py that you import, **especially if you run cells not in order**.

If you've never using Jupyter, checkout this handy article for a deep dive into the topic:

[Working efficiently with JupyterLab](https://florianwilhelm.info/2018/11/working_efficiently_with_jupyter_lab/)


## Setting up Jupyter Headless on Biowulf

1. We start an interactive Slurm session (with tunneling enabled): `sinteractive --mem=<m>g --cpus-per-task=<n> --time=24:00:00 --tunnel` . Uses <m> GB and <n> no. of processors. Default time is about 8 hours, I think. I’m using 24 hours here. I generally use 6 GB and 12 cpus, which is more than sufficient for my tasks; you can use more or less.
    1. You can make this command a bash alias by pasting this snippet in your `.bash_aliases` on your biowulf account (is it an account ?) :
    
    ```bash
    alias tunnel_server = "sinteractive --mem=12g --cpus-per-task=6 --time=24:00:00 --tunnel"
    ```
    
2. Now, simply start headless Jupyter with the aliased command `jupyter_headless` . In your `.bash_aliases` , add `alias jupyter_headless="jupyter lab --no-browser --port=$PORT1"` .
3. In your local machine’s `bash_aliases` , add `alias tunnel_client='$(ssh [biowulf.nih.gov](http://biowulf.nih.gov/) /usr/local/slurm/bin/reconnect_tunnels)'`. Reconnect to the tunnel as `tunnel_client` .

On Biowulf, the headless command outputs a URL, which can be accessed on your local machine, once the tunnels are connected.

## Setting up after aliases added

1. Log into biowulf via terminal
2. Write command `tunnel_server` and press enter
    1. Wait for your job to be allocated
3. Go to the directory from which you want to run a Jupyter Notebook 
4. Write command `jupyter_headless` and press enter
5. Open up a separate terminal that’s on your local machine
6. Write command `tunnel_client` and press enter
    1. Wait until you log in
7. Copy one of two URLs from first terminal and paste it into your browser