# App widget in compose

## News ImageAppWidget

```Kotlin

class NewsImageAppWidget : SportenAppWidget() {

    override suspend fun provideGlance(context: Context, id: GlanceId) {
        super.provideGlance(context, id)
        provideContent {
            WidgetTheme {
                ImageNewsWidget(hiltEntryPoint.newsFeedRepository())
            }
        }
    }
}

@Composable
private fun NewsImageWidget(newsFeedRepository: NewsFeedRepository) {
    val cellSize = LocalCellSize.current
    val newsFeed = remember {
        mutableStateListOf<NewsFeedEntity>()
    }
    LaunchedEffect(Unit) {
        newsFeed.clear()
        newsFeed.addAll(newsFeedRepository.getNewsFeed(10) ?: emptyList())
    }

    if (newsFeed.isNotEmpty()) {
        when {
            cellSize.height == 1 -> FillContentImageWidget(data = newsFeed)
            else -> DefaultImageWidget(data = newsFeed)
        }
    }
}
```

Each `@Composable` retains only the state that is actually needed. Everything is divided into smaller `@Composables`,
allowing for the assembly of a larger whole later on. As a result, the files that make up the widget implementations are
not overwhelmingly large, but small, neat, and clear.

---

## DefaultImageWidget

Delving deeper into the `DefaultImageWidget`, we encounter what appears to be a simple structure. This is because the
most
complex logic is concealed underneath. As a result, in the future, other developers will be able to utilize the most
essential elements through convenient interfaces.

```Kotlin
@Composable
fun DefaultImageWidget(data: List<NewsFeedEntity>) {
    val cellSize = LocalCellSize.current
    val columnsCount = when (cellSize.width) {
        in 1..3 -> 1
        in 4..Int.MAX_VALUE -> 2
        else -> 0
    }

    DefaultWidgetScaffold(
        logo = {
            TV2Logo()
        },
        topBar = {
            Category(text = stringResource(R.string.sport), color = R.color.rebrand_g6)
        }
    ) {
        Grid(
            size = GridSize(columnsCount, cellSize.height)
        ) {
            itemsSpacedBy(8.dp) { row, column ->
                val index = row * columnsCount + column
                NewsImageItem(
                    modifier = GlanceModifier.cornerRadius(8.dp),
                    data = data[index % data.size]
                )
            }
        }
    }
}
```

An example of this approach is the `DefaultWidgetScaffold` and `Grid`. At the time of writing this widget, Glance did
not
have the Grid tool, necessitating the manual implementation of a custom layout.

### DefaultWidgetScaffold

`DefaultWidgetScaffold` effectively manages the complete implementation of the widget structure, allowing other widgets
to utilize it while maintaining a consistent structure. All elements of composition like `logo`,`topBar` and `conent`
are passed via params.

```Kotlin
@Composable
fun DefaultWidgetScaffold(
    logo: @Composable () -> Unit,
    topBar: @Composable () -> Unit,
    content: @Composable () -> Unit
) {
    Column(
        modifier = GlanceModifier
            .padding(8.dp)
            .background(R.color.rebrand_g8)
            .appWidgetBackground()
    ) {
        Row(
            modifier = GlanceModifier.fillMaxWidth(),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Box(
                modifier = GlanceModifier.defaultWeight()
            ) {
                topBar()
            }
            logo()
        }
        Spacer(modifier = GlanceModifier.height(8.dp))
        Box(
            modifier = GlanceModifier.fillMaxSize()
        ) {
            content()
        }
    }
}
```

### Grid

Aside from the somewhat complex logic responsible for arranging elements in Grid, it's also worth noting GridScope. This
is a set of functions that can be used within Grid to display elements in a clear and coherent manner via simple
interface.

```Kotlin
internal interface GridScope {
    fun items(itemContent: @Composable (row: Int, column: Int) -> Unit)

    fun itemsSpacedBy(spaceBetween: Dp, itemContent: @Composable (row: Int, column: Int) -> Unit)
}

internal data class GridSize(
    val columns: Int,
    val rows: Int
)

@Composable
internal fun Grid(
    size: GridSize,
    modifier: GlanceModifier = GlanceModifier,
    content: GridScope.() -> Unit
) {
    GridImpl(
        modifier = modifier,
        content = applyGridScope(size, content)
    )
}

private fun applyGridScope(
    size: GridSize,
    content: GridScope.() -> Unit
): @Composable () -> Unit {
    var spaceBetweenItems = 0.dp
    val itemList = mutableListOf<@Composable () -> Unit>()
    val gridScopeImpl = object : GridScope {
        override fun items(itemContent: @Composable (row: Int, column: Int) -> Unit) {
            repeat(size.rows) { row ->
                repeat(size.columns) { column ->
                    itemList.add { itemContent(row, column) }
                }
            }
        }

        override fun itemsSpacedBy(
            spaceBetween: Dp,
            itemContent: @Composable (row: Int, column: Int) -> Unit
        ) {
            spaceBetweenItems = spaceBetween
            items(itemContent)
        }
    }
    gridScopeImpl.apply(content)

    return {
        Column(
            modifier = GlanceModifier
                .fillMaxSize()
        ) {
            repeat(size.rows) { row ->
                if (row != 0) {
                    Spacer(modifier = GlanceModifier.height(spaceBetweenItems))
                }
                Row(
                    modifier = GlanceModifier
                        .defaultWeight()
                        .fillMaxWidth()
                ) {
                    repeat(size.columns) { column ->
                        if (column != 0) {
                            Spacer(modifier = GlanceModifier.width(spaceBetweenItems))
                        }
                        Box(
                            modifier = GlanceModifier
                                .defaultWeight()
                        ) {
                            itemList[row * size.columns + column]()
                        }
                    }
                }
            }
        }
    }
}

@Composable
private fun GridImpl(
    modifier: GlanceModifier = GlanceModifier,
    content: @Composable () -> Unit
) {
    content()
}
```