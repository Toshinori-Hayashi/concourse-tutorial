description: The central concept of Concourse is to run tasks. You can run them directly from the command line as below, or from within pipeline jobs (as per every other section of the tutorial).
image_path: /images/build-output-hello-world.png


# Hello World

Concourseはタスクの実行を基本としています。以下に示すようにコマンドラインから直接実行も可能ですし、（他のセクションで示されるように）パイプラインから実行することも可能です。

```
git clone https://github.com/starkandwayne/concourse-tutorial
cd concourse-tutorial/tutorials/basic/task-hello-world
fly -t tutorial execute -c task_hello_world.yml
```

実行結果は以下のように始まります。

```
executing build 1 at http://127.0.0.1:8080/builds/1
initializing
```

Concourseのタスクは「コンテナ」内で実行されます（それぞれのタスクを実行するために最適なプラットフォームが利用できます）。`task_hello_world.yml`では`linux`の`busybox`コンテナ上で実行されていることがわかります。`busybox`Dockerイメージのダウンロードは最初に一度だけ行われ、以降は毎回最新の`busybox`イメージがあるかがチェックされます。


このコンテナでは、`echo hello world`コマンドが実行されています。

`task_hello_world.yml`ファイルは以下のようになります。


```yaml
---
platform: linux

image_resource:
  type: docker-image
  source: {repository: busybox}

run:
  path: echo
  args: [hello world]
```

実行されると最終的に`echo hello world`コマンドが呼び出されます。

```
running echo hello world
hello world
succeeded
```

URL http://127.0.0.1:8080/builds/1がブラウザに表示されます。それは同じ仕事の別の見方です。

<!--
The URL http://127.0.0.1:8080/builds/1 is viewable in the browser. It is another view of the same task.
-->

![build-output-hello-world](../../images/build-output-hello-world.png)

## Task Docker Images

image_resource:やを変更してrun:、別のタスクを実行してみてください：

Try changing the `image_resource:` and the `run:` and run a different task:

```yaml
---
platform: linux

image_resource:
  type: docker-image
  source: {repository: ubuntu}

run:
  path: uname
  args: [-a]
```

This task file is provided for convenience:

```
fly -t tutorial execute -c task_ubuntu_uname.yml
```

The output looks like:

```
executing build 2 at http://127.0.0.1:8080/builds/2
initializing
...
running uname -a
Linux fdfa0821-fbc9-42bc-5f2f-219ff09d8ede 4.4.0-101-generic #124~14.04.1-Ubuntu SMP Fri Nov 10 19:05:36 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
succeeded
```

The reason that you can select any base `image` (or `image_resource` when [configuring a task](http://concourse-ci.org/running-tasks.html)) is that this allows your task to have any prepared dependencies that it needs to run. Instead of installing dependencies each time during a task you might choose to pre-bake them into an `image` to make your tasks much faster.

## Miscellaneous

If you're interested in creating new Docker images using Concourse (of course you are), then there is a future section [Create and Use Docker Images](/miscellaneous/docker-images).