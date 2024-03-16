# OpenBudgeteer Documentation

## Install

``` bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Deployment

``` bash
mike deploy --push --update-aliases 1.6.x latest
mike deploy --push pre-release
```
