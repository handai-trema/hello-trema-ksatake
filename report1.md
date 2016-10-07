#情報ネットワーク学演習Ⅱ 10/5 レポート課題
============

##課題1


###課題内容



仮想スイッチを停止したら、コントローラで次のメッセージを表示するようにしてみよう:

```
Bye 0xabc
```

###解答内容


ensyuu2@ensyuu2-VirtualBox:~/hello-trema-ksatake/lib/hello_trema.rb において、以下の3行を追加した。

```
  def switch_disconnected(datapath_id)
    logger.info "Bye #{datapath_id.to_hex}"
  end
```

ここでは、switch_disconnectedというイベントが発生した際、すなわちスイッチの切断が行われた際に実行されるメソッドを定義し、当該メソッドにおいてlogger.info　を用いて、""内の情報を出力させるようにした。""内のBye #{datapath_id.to_hex}　は、スイッチ切断のイベントが発生したdatapathのIDを示している。結果的に、プログラムは以下のようになる。


```
# Hello World!
class HelloTrema < Trema::Controller
  def start(_args)
    logger.info 'Trema started.'
  end

  def switch_ready(datapath_id)
    logger.info "Hello #{datapath_id.to_hex}!"
  end
  def switch_disconnected(datapath_id)
    logger.info "Bye #{datapath_id.to_hex}"
  end

end
```


##課題2


###課題内容



HelloTrema が起動したら次のメッセージを表示するようにしてみよう:

```
HelloTrema started.
```
ただし、次の回答ではダメ (なぜダメか？も考察しよう)

```
class HelloTrema < Trema::Controller
  def start(_args)
    logger.info 'HelloTrema started.'
  end
  ...

```


###解答内容

以下の1行に変更を加えた。

変更前
```
logger.info 'Trema started.'
```


変更後
```
logger.info self.name+' started.'
```

ここでは、Tremaと単純に出力されていた箇所を
self.nameに変更することで、この記述を行ったクラスの名前を出力させるようにした。結果的にプログラムは以下のようになる。


```
# Hello World!
class HelloTrema < Trema::Controller
  def start(_args)
    logger.info self.name+' started.'
  end

  def switch_ready(datapath_id)
    logger.info "Hello #{datapath_id.to_hex}!"
  end
  def switch_disconnected(datapath_id)
    logger.info "Bye #{datapath_id.to_hex}"
  end

end
```


また、単純に、HelloTremaと文字列を書き換えるだけで良くない理由は主に保守性にある。仮にこのクラス名を変更する必要が生じ、変更を加えた場合、私の提示した書き方であれば、出力文字列も変更後のクラス名に変わるが、単純に、HelloTremaと加えただけではクラス名変更時に、こちらも変更することを失念すると、これはバグの原因となってしまう。そのため、この書き方は不十分である。

