# OpenBudgeteer Documentation

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
