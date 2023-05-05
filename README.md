# pyspark-demo

## Requirements

- Docker desktop

## Running PySpark

To spin up a PySpark instance, run the following at the terminal:

```bash
docker-compose up -d
```

To tear down the container, run the following at the terminal:

```bash
docker-compose down
```

## Tracking notebooks with version control

It is considered good practice to not track notebook outputs with git, as they change after every run. To still keep the source code inside the cells, the notebook outputs can be stripped before being committed. For this project I used [jq](https://stedolan.github.io/jq/).

After having installed jq locally, add the following to your git config:

```properties
[filter "nbstrip_full"]
    clean = "jq --indent 1 \
            '(.cells[] | select(has(\"outputs\")) | .outputs) = []  \
            | (.cells[] | select(has(\"execution_count\")) | .execution_count) = null  \
            | .metadata = {\"language_info\": {\"name\": \"python\", \"pygments_lexer\": \"ipython3\"}} \
            | .cells[].metadata = {} \
            '"
    smudge = cat
    required = true

```

This, combined with the [.gitattributes](.gitattributes)-file will result in this being run automatically.

[To read more, check out this blog post.](https://timstaley.co.uk/posts/making-git-and-jupyter-notebooks-play-nice/)
