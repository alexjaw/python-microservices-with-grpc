# Next: Docker compose

# 17 sept 2023 Production-ready python microservice
* Build and test run recommendations docker `recommendations`
* ...marketplace docker `marketplace`
* Create docker network `microservices`
* Update docker `marketplace`
* Run the docker services and test that marketplace can communicate with recommendations - Yes!

# 17 sept 2023 Tying It Together
Here we run 2 microservices:
1. `recommendation/recommendations.py` which provides book recommendations.
1. `marketplace/marketplace.py`which is a Flask app that displays recommendations in the web browser.
```
python -m grpc_tools.protoc \
    -I ../protobufs \
    --python_out=. \
    --grpc_python_out=. \
    ../protobufs/recommendations.proto
```
## ImportError: cannot import name 'soft_unicode' from 'markupsafe'
Fix with downgrading markupsafe (updated `requirements.txt`).
```
pip install markupsafe==2.0.1
```

# 14 sept 2023 Initial setup
* Skipping docker for now
* created venv
* installed recommendations/requirements.txt
```
(venv) gitpod /workspace/python-microservices-with-grpc (my-test) $ pip list
Package          Version
---------------- -------
attrs            23.1.0
grpc-interceptor 0.11.0
grpcio           1.58.0
grpcio-tools     1.58.0
more-itertools   10.1.0
packaging        23.1
pip              23.2.1
pluggy           0.13.1
protobuf         4.24.3
py               1.11.0
pytest           5.4.3
setuptools       65.5.0
wcwidth          0.2.6
```
## Compile protobuf
```
python -m grpc_tools.protoc \
    -I ../protobufs \
    --python_out=. \
    --grpc_python_out=. \
    ../protobufs/recommendations.proto
```
## Test initial recommentations
* Split into 2 terminals
* terminal 1 starts the server
```
(venv) gitpod /workspace/python-microservices-with-grpc (my-test) $ cd recommendations
(venv) gitpod /workspace/python-microservices-with-grpc/recommendations (my-test) $ python recommendations.py 

```
* terminal 2 runs client code
```
(venv) gitpod /workspace/python-microservices-with-grpc/recommendations (my-test) $ python recommendations_test.py 
recommendations {
  id: 2
  title: "Murder on the Orient Express"
}
```
