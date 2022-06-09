Notes on running:
- `docker run --rm -it -p 8888:8888 jupyter jupyter notebook --ip 0.0.0.0 --allow-root --no-browser`
- must use `-p` flag for docker, not `--expose`
- must use `ip` flag for jupyter, default is localhost which is unavailable outside container
