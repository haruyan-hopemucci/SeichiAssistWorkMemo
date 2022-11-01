# Home機能の拡張

## homeコマンドの拡張

## homeGUIの拡張

### objectをcase class 化

### ページネーション

コード退避

```scala
    // ページ間移動メニュー
    val paginationPart = {
      import environment._
      Map(
        ChestSlotRef(5, 0) -> CommonButtons.openStickMenu,
        ChestSlotRef(5, 8) -> CommonButtons.transferButton(
          new SkullItemStackBuilder(SkullOwners.MHF_ArrowRight),
          s"2ページ目へ",
          HomeMenu
        )
      )
    }
```