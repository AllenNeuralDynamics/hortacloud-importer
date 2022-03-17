# tiff2octree
a tiff-to-octree converter for dask

## Initial Setup
1. Install miniconda (https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html)
2. Clone pyktx (https://github.com/JaneliaSciComp/pyktx)
3. Run ```conda env create -f environment.yml```
4. Run ```conda activate octree```
5. Run ```pip install /path/to/pyktx```

## Usage
```
commandline arguments:
  -h, --help                              show this help message and exit
  -n NUMBER, --thread NUMBER              number of threads (default: 16)
  -i INPUT, --inputdir INPUT              input directory
  -f FILE, --inputfile FILE               input image stack
  -o OUTPUT, --output OUTPUT              output directory
  -l LEVEL, --level LEVEL                 number of levels
  -c CHANNEL, --channel CHANNEL           channel id
  -d DOWNSAMPLE, --downsample DOWNSAMPLE  downsampling method: 2ndmax, area, aa (anti-aliasing), spline
  -m, --monitor                           activate monitoring. 
                                          you can see the dask dashboard at 
                                          http://(NodeName).int.janelia.org:8989/status
                                          (e.g. http://h07u01.int.janelia.org:8989/status)
  
  --origin ORIGIN       position of the corner of the top-level image in nanometers
  --voxsize VOXSIZE     voxel size of the top-level image
  --memory MEMORY       memory amount per thread (for LSF cluster)
  --project PROJECT     project name (for LSF cluster)
  --maxjobs MAXJOBS     maximum jobs (for LSF cluster)
  --lsf                 use LSF cluster
  --ktx                 generate ktx files
  --ktxout KTXOUT       output directory for a ktx octree
  --cluster CLUSTER     address of a dask scheduler server


examples: 

1. use a local cluster.
python3 tiff2octree.py -i /input_slices/tiff -l 3 -o /output/octree -d 2ndmax

2. use a LSF cluster.
python3 tiff2octree.py -i /input_slices/tiff -l 3 -o /output/octree -d 2ndmax --lsf --project scicompsoft --memory 12GB --maxjobs 10

3. output a ktx octree without a tiff octree.
python3 tiff2octree.py -i /input_slices/ch1,/input_slices/ch2 -l 3 --ktx --ktxout /output/octree/ktx -d 2ndmax

4. specify a cluster by its address.
python3 tiff2octree.py -i /input_slices/tiff -l 3 -o /output/octree --cluster tcp://10.60.0.223:8786 -d spline
```
