# 17 sept 2023 Docker tests
Use docker-compose and pytest

Error:
`E   TypeError: required field "lineno" missing from alias` 

Turnes out that pytest must be updated for python 3.11 that I currently use in gitpod. So, first update requirements.txt files and the run `docker-compose build`. After that tests worked as expected.
```
# Observe that you first must start the services in another terminal:
# gitpod /workspace/python-microservices-with-grpc (my-test) $ docker-compose up
(venv) gitpod /workspace/python-microservices-with-grpc/recommendations (my-test) $ docker-compose exec recommendations pytest recommendations_test.py 
============================ test session starts ============================
platform linux -- Python 3.11.5, pytest-7.4.2, pluggy-1.3.0
rootdir: /service/recommendations
collected 1 item                                                            

recommendations_test.py .                                             [100%]

============================= 1 passed in 0.05s =============================
(venv) gitpod /workspace/python-microservices-with-grpc/recommendations (my-test) $ docker-compose exec marketplace pytest marketplace_integration_test.py
 
============================ test session starts ============================
platform linux -- Python 3.11.5, pytest-7.4.2, pluggy-1.3.0
rootdir: /service/marketplace
collected 1 item                                                            

marketplace_integration_test.py .                                     [100%]

============================= 1 passed in 0.03s =============================
```
# 17 sept 2023 Docker compose
Docker compose allows you to work with your services from one yaml file.
* Create the yaml file
* Bring up with `docker-compose up` - voilà!
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
