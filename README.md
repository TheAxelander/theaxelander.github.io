# OpenBudgeteer Documentation

## Install

``` bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Deployment

``` bash
mike deploy --push --update-aliases x.x.x latest
mike deploy --push pre-release
```

## In case of damage

``` bash
mike delete --all --push
mike set-default --push latest
mike deploy --push --update-aliases x.x.x latest
mike deploy --push pre-release
```
