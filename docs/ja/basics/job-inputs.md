description: Pass external resources into Concourse job tasks
image_path: /images/task-docker-image-and-run-tests.png

# Using Resource Inputs in Job Tasks
# ジョブタスクでリソース入力を使用する

パイプラインでのユニットテストの実行については、今後のセクションで詳しく説明します。

Note, the topic of running unit tests in your pipeline will be covered in more detail in future sections.

単体テストを持つ単純なアプリケーションを考えてみましょう。これらのテストをパイプライン内で実行するには、次のものが必要です。

Consider a simple application that has unit tests. In order to run those tests inside a pipeline we need:

---

* `image`依存関係を含むタスク
* `resource`テストの実行方法を知っているタスクスクリプトを含む入力
* `resource`アプリケーションソースコードを含む入力

---

* a task `image` that contains any dependencies
* an input `resource` containing the task script that knows how to run the tests
* an input `resource` containing the application source code

---

Goアプリケーション [simple-go-web-app](https://github.com/cloudfoundry-community/simple-go-web-app)の例では、タスクイメージにGoプログラミング言語が含まれている必要があります。https://hub.docker.com/_/golang/（https://imagelayers.io/?images=golang:1.9-alpineのレイヤーのサイズを参照）の`golang:1.9-alpine`画像を使用します。

For the example Go application [simple-go-web-app](https://github.com/cloudfoundry-community/simple-go-web-app), the task image needs to include the Go programming language. We will use the `golang:1.9-alpine` image from https://hub.docker.com/_/golang/ (see https://imagelayers.io/?images=golang:1.9-alpine for size of layers)

---

The task file `task_run_tests.yml` includes:

```yaml
image_resource:
  type: docker-image
  source: {repository: golang, tag: 1.9-alpine}

inputs:
- name: resource-tutorial
- name: resource-app
  path: gopath/src/github.com/cloudfoundry-community/simple-go-web-app
```

The `resource-app` resource will place the inbound files for the input into an alternate path. By default we have seen that inputs store their contents in a folder of the same name. The reason for using an alternate path in this example is specific to building & testing Go language applications and is outside the scope of the section.

To run this task within a pipeline:

```
cd ../job-inputs
fly -t tutorial sp -p simple-app -c pipeline.yml
fly -t tutorial up -p simple-app
```

View the pipeline UI http://127.0.0.1:8080/teams/main/pipelines/simple-app and notice that the job automatically starts.

![trigger-job-input](/images/trigger-job-input.png)

The job will pause on the first run at `web-app-tests` task because it is downloading the `golang:1.9-alpine` image for the first time.

The `web-app-tests` output below corresponds to the Go language test output (in case you've not seen it before):

```
ok  	github.com/cloudfoundry-community/simple-go-web-app	0.003s
```

![task-docker-image-and-run-tests](/images/task-docker-image-and-run-tests.png)
