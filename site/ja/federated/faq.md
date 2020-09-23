# よくある質問

## TensorFlow Federated は、携帯電話などの実稼働設定で使用できますか？

現時点では使用できません。TFF は、実際のデバイスへのデプロイを念頭に設計されてはいますが、現段階では、この目的で提供されているツールはありません。現行リリースは、新しいフェデレーションアルゴリズムの表現や、フェデレーテッドラーニングを独自のデータセットで試すなど、含まれているシミュレーションランタイムを使用して実験的に使用することを目的としています。

いずれは、TFF にかかわるオープンソースエコシステムが進化して、物理的なデプロイプラットフォームをターゲットとするランタイムが含まれると予想しています。

## 大規模なデータセットを実験するには、TFF をどのように使用しますか？

TFF の初期リリースに含まれているデフォルトのランタイムは、チュートリアルに説明されているような小規模な実験のみを目的としています。チュートリアルの実験では、すべてのデータ（すべてのシミュレーションされるクライアント）は単一のマシンのメモリに同時に収まるため、実験全体を Colab ノートブック内でローカルに実行することができます。

ロードマップには、近い将来、非常に大規模なデータセットと多数のクライアントを使用した実験用の高性能ランタイムが含まれています。

## TFF のランダム性が期待値に一致するようにするにはどうすればよいですか？

TFF には、フェデレーテッドコンピューティングがコアに含まれるため、TFF のライターは、どこでどのように TensorFlow `Session` が開始されるのか、これらのセッションで `run` が呼び出されるのかに対する制御を行う必要はありません。ランダム性のセマンティックは、シードが設定される場合に、TensorFlow `Session` の開始と終了に依存できます。TF 1.14 の `tf.random.experimental.Generator` などを使用して、TensorFlow 2 スタイルのランダム性を使用することをお勧めします。これは、`tf.Variable` を使用して、内部状態を管理します。

期待値を管理できるように、TFF はそれがシリアル化する TensorFlow で、グラフレベルのシードではなく、演算レベルのシードを設定できるようにしています。これは、演算レベルのシードのセマンティックが、TFF でより明確である必要があるためです。`tf_computation` としてラッピングされる関数が呼び出されるたびに確定的なシーケンスが生成され、この呼び出し内でのみ、疑似乱数ジェネレータによる保証が保持されます。これは、eager モードで `tf.function` を呼び出すセマンティックとまったく同じではないことに注意してください。TFFは、`tf_computation` が呼び出されるたびに一意の `tf.Session` を開始して終了しますが、eager モードで関数を繰り返し呼び出すことは、同じセッション内で出力テンソルに対して繰り返し `sess.run` を呼び出すのに変わりません。

## どうすれば貢献できますか？

[README](../README.md) および[貢献者向けガイドライン](../CONTRIBUTING.md)をご覧ください。