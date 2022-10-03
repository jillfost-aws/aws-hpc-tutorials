+++
title = "e. Auto execute pipeline"
date = 2021-09-30T10:46:30-04:00
weight = 70
tags = ["tutorial", "DeveloperTools", "CodePipeline", "CodeBuild", "CI/CD"]
+++

In this section, we will update the sample Dockerfile created earlier to automatically trigger the container build and update to Amazon ECR as part of the CodePipeline we created earlier.

We will modify the Dockerfile to add extra scripts and input files that we will need to run a full example in the next lab.

1. First confirm you are in the **MyDemoRepo** repository:

```
pwd # should be MyDemoRepo
```

2. Update the Dockerfile to the following. 

```bash
cat > Dockerfile << EOF
FROM spack/amazon-linux:v0.18.0 as build
# Add our spack.yaml file that defines our build and environment
ADD spack.yaml /opt/spack-environment/spack.yaml
# Set up spack env & binary cache, then build the software
RUN spack env activate -d /opt/spack-environment \
&&  spack mirror add binary_mirror https://binaries.spack.io/releases/v0.18 \
&&  spack buildcache keys --install --trust \
&&  spack install --reuse --use-cache --fail-fast \
&&  spack gc -y \
&&  spack find -v
# Create a script to activate the spack environment on load
RUN spack env activate --sh -v -d /opt/spack-environment > /etc/profile.d/z10_spack_environment.sh 
ENTRYPOINT [ "/bin/bash", "-l" ]

# A multi-stage build can make images smaller and more efficient by keeping only files we need
FROM public.ecr.aws/amazonlinux/amazonlinux:latest as final
COPY --from=build /opt/software /opt/software
COPY --from=build /opt/view /opt/view
COPY --from=build /etc/profile.d/z10_spack_environment.sh /etc/profile.d/z10_spack_environment.sh
# Add yum requirements
RUN yum -y install \
      unzip \
      libgomp \
      libatomic \
&&  yum clean all \
&&  rm -rf /var/cache/yum
# Add Gromacs launcher script and data files
ADD ./inputs /inputs
ADD ./entrypoint.sh /entrypoint.sh
# Setup run script
RUN chmod +x /entrypoint.sh \
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "all" ]
WORKDIR /
EOF
```

3. We will need some extra files to finish out our container.  
  - The **entrypoint.sh** script will handle setting up our environment and executiong GROMACS commands for us.
  - The **inputs/** files contain a set of default inputs that we will use.  These files will be used to execute the "Lysozyme in Water" described on the [GROMACS tutorials website](http://www.mdtutorials.com/gmx/lysozyme/index.html).

You will copy these files from the S3 bucket.

  ```bash
  aws s3 cp s3://sc22-hpc-labs/entrypoint.sh .
  aws s3 cp -r s3://sc22-hpc-labs/inputs .
  ```

4. Now we will update and push this file to the created codecommit repository
```bash
git add Dockerfile entrypoint.sh inputs
git commit -m "Updated image to with pipeline scripts and inputs"
git push origin main
```

5. In the AWS Management Console search bar, type and select **CodePipeline**. Click on the **MyDemoPipeline** that you created in the previous section. You should now see that the CodeCommit push above should have triggered the build via CodeBuild automatically. 
![AWS CodePipeline](/images/cicd/codepipeline-6.png)

6. Click on the **Details** deep link from the Build stage of the CodePipeline. This will take you to build logs from the CodeBuild project that you created:

![AWS CodePipeline](/images/cicd/codepipeline-7.png)
![AWS CodePipeline](/images/cicd/codepipeline-8.png)


7. Click on the **Tail logs** to see the on-going or completed build process. This is showcasing every step of the build process as provided in your **buildspec.yml** file.
![AWS CodePipeline](/images/cicd/codepipeline-9.png)

8. In addition to the build the pipeline is also pushing the built container image to the container registry in Amazon ECR. 

Once the build has completed you should be able to see that a container exists in the ECR repository tagged as **latest**:
1. Open the [Amazon ECR console](https://us-east-2.console.aws.amazon.com/ecr/repositories).
2. Select **sc22-container**

You should see an entry like:

**SCREENSHOT MISSING**