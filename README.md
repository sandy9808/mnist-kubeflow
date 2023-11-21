
Clone this repo for running kubectl commands in the local terminal.
1. Apply the configuration with the following command:

    ```bash
    kubectl apply -f access_kfp_from_jupyter_notebook.yaml
    ```

2. Spinning up a new Notebook Instance
Head to notebooks in kubeflow dashboard. For the container image, select `jupyter-tensorflow-full:v1.8.0`.

3. Clone this repo in the notebook server terminal.

Update Python Packages

install following packages:
  For installing kfp: 
  pip install "cython<3.0.0"
  pip install --no-build-isolation kfp==1.8.12
kfp-pipeline-spec        0.1.13
kfp-server-api           1.8.2
kserve                   0.11.2 (not getting installed)

3. Setup MinIO for Object Storage

Kubeflow has already setup a MinIO tenant
In order to get access to MinIO from outside of your Kubernetes cluster and check the bucket, do a port-forward:
Run this command in local terminal:
```bash
kubectl port-forward -n kubeflow svc/minio-service 9000:9000
```
Then you can access the MinIO dashboard at http://localhost:9000 and check the bucket name or create your own bucket. Alternatively, you can use the MinIO CLI Client
Default values should be:
accesskey: minio
secretkey: minio123
bucket: mlpipeline

Run this command to get the ip address of the endpoint of minio.
Run this command in local terminal.
```bash
kubectl describe svc/minio-service -n kubeflow
```
<img width="670" alt="image" src="https://github.com/sandy9808/mnist-kubeflow/assets/58395595/43c53a3a-fb71-4193-8ded-1b8cd49343f2">
Replace this ip address with the ip address written in all notebooks wherever minio_client is used.

4. Execute all the cells of digits_recognizer_notebook.ipynb after ensuring minio can be accessed from this notebook.

5. Set minIO secret for kserve
   run following command in local terminal.
```bash
kubectl apply -f set-minio-kserve-secret.yaml
```

6. Create a ML pipeline with Kubeflow Pipelines
Execute all the cells in digits_recognizer_pipeline.ipynb. Ensure all the pipelines are running without error.

7. Create an endpoint to get the link of the served model.
   To do this execute this command in local terminal:
  ```bash
kubectl apply -f create_kserve_inference.yaml
```

7. Testing the model inference
   execute kserve_python_test.ipynb to test it.
