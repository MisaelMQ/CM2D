FROM pytorch/pytorch

WORKDIR /workspace

ADD . /workspace

RUN pip install -r requirements.txt

CMD [ "python" , "/workspace/app.py" ]

RUN chown -R 42420:42420 /workspace

ENV HOME=/workspace
