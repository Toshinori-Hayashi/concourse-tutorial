description: Review the four ways to trigger a job.

# Trigger Jobs / ジョブの開始
<!-- # Trigger Jobs -->

ジョブを開始するには、次の4つの方法があります。


---

* （以前のセクションで行ったように）Web UI上のジョブで `+`　ボタンをクリックする
* ジョブを開始するリソースを入力します（次のレッスン [Triggering Jobs with Resources](/basics/triggers/)を参照してください）
* `fly trigger-job -j pipeline/jobname` コマンド
* Concourse APIへ `POST` で送信する


---

私たちは`hello-world`パイプラインを再開させることができます`job-hello-world`：

We can re-trigger our `hello-world` pipeline's `job-hello-world`:

---

```
fly -t tutorial trigger-job -j hello-world/job-hello-world
```

---

ジョブが実行されている間に、完了したら、次のコマンドを使用して端末の出力を見ることができます`fly watch`。

Whilst the job is running, and after it has completed, you can then watch the output in your terminal using `fly watch`:

```
fly -t tutorial watch -j hello-world/job-hello-world
```

---

あるいは、2つのコマンドを組み合わせることもできます。ジョブをトリガし、`trigger-job -w`フラグを指定して出力を監視します。

Alternately, you can combine the two commands - trigger the job and watch the output with the `trigger-job -w` flag:

```
fly -t tutorial trigger-job -j hello-world/job-hello-world -w
```

---

次のレッスンでは、入力リソースの変更後にジョブをトリガーする方法を学習します。

In the next lesson we will learn to trigger jobs after changes to an input resource.
