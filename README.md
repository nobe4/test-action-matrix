# test-action-matrix

POC to use a file as source for the `strategy:matrix`.

It works in 2 steps:

1. [setup](https://github.com/nobe4/test-action-matrix/blob/master/.github/workflows/test.yml#L7-L17)

   Loads a matrix from [`matrix.json`](matrix.json) and set it as one of its output.
    
2. [run](https://github.com/nobe4/test-action-matrix/blob/master/.github/workflows/test.yml#L19-L25)

   Runs after `setup` is done and use `fromJson()` to parse `setup`'s output into a mapping, which then gets used for the runs.

**Result**:

![image](https://user-images.githubusercontent.com/2452791/153205223-478b6de3-b58f-42d5-9692-21887988b32b.png)

**Notes**: The JSON has to be minified before the `echo "::set-output name=matrix::..."`. Here I've minified [`matrix.json`](matrix.json) but it's also possible to do it in the action like so:

```yaml
run: |
    content=`cat ./matrix.json`
    content="${content//'%'/'%25'}"
    content="${content//$'\n'/'%0A'}"
    content="${content//$'\r'/'%0D'}"
    echo "::set-output name=matrix::$content"
```
