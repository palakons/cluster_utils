# Utilities for running on vll cluster

    def getLocalPath(local_storage, path, clone=True)
 
Returns the path to the actual file depending on where the code is run.

  If run locally (not vision-0x), just return the path.


  If run on the cluster:
    Copy the file (or the entire folder) specified by "path" from your
    workstation to the cluster and return the local path to the copied
    file on the cluster. Copying is performed when the file doesn't
    exist or when the copied file is too old.

  *** Setting the "clone" flag to False means skipping the copying,
  and only creating the directory tree on the cluster. This is for creating
  an output file / folder.

  Suppose the file on your workstation is:
  
    /home/supasorn/research/project1/data/train.txt
    
  The copied file on cluster will be:
  
    LOCAL_STORAGE/home/supasorn/research/project1/data/train.txt
    
  where LOCAL_STORAGE is the first argument provided to this function.


  ## Usage:
    Go to your research folder, run:
        git clone https://github.com/supasorn/cluster_utils.git
    This will create a folder cluster_utils/ inside your research folder.

    Then in your code:
        from cluster_utils.localpath import getLocalPath


  ## Examples:
    1. If you want to have large data file available to your cluster locally,
    do the following. Suppose the data is "data/bigdata.tfrecord"
    (relative path)

    Instead of using:
      file_path = "data/bigdata.tfrecord"
    Use:
      file_path = getLocalPath("/home2/YOURUSER/local_storage", "data/bigdata.tfrecord")

    This also works with absolute paths on your workstation.


    2. If you want to write an output file locally instead of sshfs-mounted
    directory on your workstation, set the clone flag to false:

      getLocalPath("/home2/YOURUSER/local_storage", "output/out.txt", clone=False)


    3. If you want to have the entire folder cloned from your workstation.

      getLocalPath("/home2/YOURUSER/local_storage", "model_dir/")


    4. If you want to use an output folder locally, e.g., when using tensorflow's
    estimator which requires setting model_dir. Set clone to False and use:

      getLocalPath("/home2/YOURUSER/local_storage", "run/experiment1", clone=False)


  ## Tips:
    Usually the local_storage has to be set for every getLocalPath()'s calls, but
    we can shorten the call with a python's lambda function.

    lp = lambda path, clone=True: getLocalPath("/home2/YOURUSER/local_storage", path, clone)

    And instead of using:
      file_path = getLocalPath("/home2/YOURUSER/local_storage", "data/bigdata.tfrecord")
    we can use:
      file_path = lp("data/bigdata.tfrecord")

