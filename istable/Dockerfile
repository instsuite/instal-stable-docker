
FROM instal-clingo
COPY . /code
WORKDIR /code

WORKDIR ../code/instal-stable
RUN ls
RUN cp /clingo-5.1.0/build/release/python/clingo.so ../code/instal-stable/instal/clingo.so
RUN pip install -r ../code/instal-stable/requirements.txt
RUN ls
WORKDIR ../code/instal-stable
RUN ls ./instal
RUN pip install . --force




ENTRYPOINT ["python3", "launcher.py"]