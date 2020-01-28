# Running Blender on Dis.co's Distributed Platform 

With over half a million downloads per month, Blender is one of the most popular, free and open-source 3D creation tools in the market today. In this sample code, we will focus on the integration of Cycles, a ray tracing based engine, on the [Dis.co](http://dis.co) and Packet platform. Dis.co is a parallelization platform that provides easy and flexible ways to scale your compute solutions by automatically offloading tasks onto servers on-demands. On the other hand, Packets is a bare-metal server solution for accessing high-performance computers in minutes. By combining the Dis.co and Packet platform, we are exploring the best of parallelization and best of high-performance bare-metal server configurations for maximal throughputs.

![Classroom Output](https://github.com/Iqoqo/disco_blender/blob/master/classroom_sample/classroom.gif "Classroom")

## A Blender-enabled Docker Image
To support Blender on Dis.co, a custom Docker image (with Blender 2.81a pre-installed) is created and linked to the dis.co account. You can preview the docker image by downloading it from the DockerHub. 

```
docker pull raymondlo84/disco_blender
```

And once completed, you can see the image with the docker command.

```
docker images

REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
raymondlo84/disco_blender   latest              a667592ba964        3 weeks ago         851MB

```


## Blender Core Python Script

The core python script provides a wrapper to execute blender render program in parallel across all servers with dis.co platform. 

## Job Generator
We provide a simple script to generate a parallelized the blender rendering tasks. To run the job generator, we execute the python with your favourite IDE or at the terminal. 

```
python job_generator.py
```
By running the script, it will generate a set of files which will be uploaded to the dis.co server.
```
   8 -rw-r--r--   1 raymondlo84  staff       77 Nov 27 09:00 classroom_1_10.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_11_20.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_21_30.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_31_40.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_41_50.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_51_60.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_61_70.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_71_80.txt
   8 -rw-r--r--   1 raymondlo84  staff       78 Nov 27 09:00 classroom_81_90.txt
   8 -rw-r--r--   1 raymondlo84  staff       79 Nov 27 09:00 classroom_91_100.txt
   8 -rw-r--r--   1 raymondlo84  staff       80 Nov 27 09:00 classroom_101_110.txt
   8 -rw-r--r--   1 raymondlo84  staff       80 Nov 27 09:00 classroom_111_120.txt
   8 -rw-r--r--   1 raymondlo84  staff       80 Nov 27 09:00 classroom_121_130.txt
```

For example, if we look inside the classroom_111_120.txt, you will see 4 lines of text.

```
http://100.25.247.222/uploads/classroom4k.zip
classroom/classroom.blend
111
120
```

The first line is the URL to the blender project, in a zip format. It is important that we package all dependencies and all other asset files inside that folder. This will be distributed across all servers in runtime. 
The second line is the path to the blender project.
The third and forth line define the range for the frames in and out. In example, classroom_111_120.txt will render frame 111 to 120 from the blender project.

## How to Run (CLI)
Once you have generated the tasks files (e.g., classroom_1_10.txt, classroom_11_20.txt, ... etc), we can now run a new job on the dis.co server with the command line interface.

1. Login with the command (Please contact us to obtain the API key for the account) 

```
disco login -k [API key]
```

2. Add and Run the job (also make note of the <job_id> for next step).

```
 disco job create -cit l --name "blender_example" --script blender_core.py --input "classroom_*.txt" -r
```

3. Monitor the job with the disco's view command: <job_id> 

```
disco job view <job_id> 
```

4. Download the results (once the job is completed)

```
disco job download-results <job_id> -d .
```

## How to Run (Dis.co Web UI)

Note: Please talk to our dis.co team and enable a dis.co account with the customized docker image.

1. Open <app.dis.co> in a Chrome browser (Currently we do not support other browsers)
2. Login to the dis.co account with your account username and password
3. Click "Create A Job" button (right corner)
4. Select (or drag and drop) the script file (e.g., **blender_core.py**)
5. Select (or drag and drop) a list of tasks (e.g., **classroom_1_10.txt**, **classroom_11_20.txt**,...)
6. Select "Large" as the Job Size from the drop down list
7. Check the "Autorun this job" checkbox
8. Click "Create Job" button


Once the job is completed, you can download individual results from the web interface. 



