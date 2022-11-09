# issue#281

## メニュー2ページ目表示時にエラー

```text
[09:40:17 INFO]: メニューのクリックを非同期で処理する最中にエラーが発生しました。
[09:40:17 WARN]: java.util.NoSuchElementException: None.get
[09:40:17 WARN]:  at scala.None$.get(Option.scala:627)
[09:40:17 WARN]:  at scala.None$.get(Option.scala:626)
[09:40:17 WARN]:  at com.github.unchama.datarepository.bukkit.player.PlayerDataRepository$$anon$1.apply(PlayerDataRepository.scala:30)
[09:40:17 WARN]:  at com.github.unchama.seichiassist.subsystems.gacha.System$$anon$1$$anon$2.consumeGachaTicketAmount(System.scala:134)
[09:40:17 WARN]:  at com.github.unchama.seichiassist.subsystems.gacha.System$$anon$1$$anon$2.consumeGachaTicketAmount(System.scala:89)
[09:40:17 WARN]:  at com.github.unchama.seichiassist.menus.stickmenu.SecondPage$ButtonComputations.computeBulkDrawGachaButton(SecondPage.scala:354)
[09:40:17 WARN]:  at com.github.unchama.seichiassist.menus.stickmenu.SecondPage$.computeMenuLayout(SecondPage.scala:95)
[09:40:17 WARN]:  at com.github.unchama.seichiassist.menus.stickmenu.SecondPage$.computeMenuLayout(SecondPage.scala:49)
[09:40:17 WARN]:  at com.github.unchama.menuinventory.Menu.$anonfun$open$4(Menu.scala:44)
[09:40:17 WARN]:  at flatMap @ com.github.unchama.menuinventory.Menu.$anonfun$open$3(Menu.scala:43)
[09:40:17 WARN]:  at map @ com.github.unchama.minecraft.bukkit.actions.OnBukkitServerThread.$anonfun$runAction$9(OnBukkitServerThread.scala:51)
[09:40:17 WARN]:  at cancelable @ com.github.unchama.minecraft.bukkit.actions.OnBukkitServerThread.$anonfun$runAction$5(OnBukkitServerThread.scala:36)
[09:40:17 WARN]:  at flatMap @ com.github.unchama.minecraft.bukkit.actions.OnBukkitServerThread.$anonfun$runAction$5(OnBukkitServerThread.scala:29)
[09:40:17 WARN]:  at flatMap @ com.github.unchama.minecraft.bukkit.actions.OnBukkitServerThread.runAction(OnBukkitServerThread.scala:25)
[09:40:17 WARN]:  at as @ com.github.unchama.targetedeffect.player.PlayerEffects$.$anonfun$openInventoryEffect$1(PlayerEffects.scala:23)
[09:40:17 WARN]:  at flatMap @ com.github.unchama.menuinventory.Menu.$anonfun$open$2(Menu.scala:42)
[09:40:17 WARN]:  at flatMap @ com.github.unchama.menuinventory.Menu.$anonfun$open$1(Menu.scala:41)
[09:40:17 WARN]:  at <* @ com.github.unchama.fs2.workaround.fs3.Fs3Topic$$anon$2.$anonfun$subscribeAwait$2(Fs3Topic.scala:182)
[09:40:17 WARN]:  at <* @ com.github.unchama.fs2.workaround.fs3.Fs3Topic$$anon$2.$anonfun$subscribeAwait$2(Fs3Topic.scala:182)
[09:40:17 WARN]:  at apply @ fs2.Stream$.runStream$1(Stream.scala:1859)
[09:40:17 WARN]:  at <* @ com.github.unchama.fs2.workaround.fs3.Fs3Topic$$anon$2.$anonfun$subscribeAwait$2(Fs3Topic.scala:182)
[09:40:17 WARN]:  at *> @ com.github.unchama.menuinventory.slot.button.Button.$anonfun$effectOn$2(Button.scala:36)
```

```scala:consumeGachaTicketAmount
              override def toggleConsumeGachaTicketAmount: Kleisli[F, Player, Unit] = Kleisli {
                player =>
                  ContextCoercion(
                    gachaDrawSettingRepository(player).toggleConsumeGachaTicketAmount() // ココのgachaDrawSettingRepository
                  )
              }

```

```scala:
trait PlayerDataRepository[R] extends KeyedDataRepository[Player, R] {

  override def apply(player: Player): R

}

object PlayerDataRepository {

  def unlift[R](f: Player => Option[R]): PlayerDataRepository[R] =
    new PlayerDataRepository[R] {
      override def apply(player: Player): R = f(player).get  // ココのgetがNoneになる

      override def isDefinedAt(x: Player): Boolean = f(x).isDefined
    }

```