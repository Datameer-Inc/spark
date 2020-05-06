The following commands can be used to build the spark image we wanna use on Google's GKE.


# source datameer env vars
source ./dev/export-datameer-env.sh

# build distribution
./dev/make-distribution.sh --name ${__distSuffix} --tgz -P${__vHadoop} -Pkubernetes -P${__vScala}

# extract distribution
tar -xzf spark-2.4.2-bin-${__distSuffix}.tgz && cd spark-2.4.2-bin-${__distSuffix}

# build image
bin/docker-image-tool.sh -n -m -t ${__dockerTag} build

# tag and push image
docker tag spark:${__dockerTag} ${__dockerRegistry}/spark:${__dockerTag}
docker push ${__dockerRegistry}/spark:${__dockerTag}

