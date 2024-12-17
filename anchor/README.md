- 使用Anchor时，将在`anchor init`期间创建一个`keypair`对，并且私钥保存在项目的`target/deploy`目录中。

- `#[program]`标注的每一个module都是不同的instruction handlers，属于不同的部分

- 传参规则：
  - 由于`Example`需要第一个和第三个参数，因此`example_instruction`的三个参数都需要传入
  - 如果`Example`需要的是第一个和第二个参数，并且`input_three`没有被使用，那么在调用`example_instruction`的时候可以不传入`input_three`

```rust
pub fn example_instruction(
    ctx: Context<Example>,
    input_one: String,
    input_two: String,
    input_three: String,
) -> Result<()> {
    ...
    Ok(())
}
 
#[derive(Accounts)]
#[instruction(input_one:String, input_two:String)]
pub struct Example<'info> {
    ...
}
```

- As with `init`, you must include `system_program` as one of the accounts in the account validation struct when using `realloc`.
- 和`init`一样，在使用`realloc`的时候，你必须使用`system_program`作为其中一个账户作为验证
- `#[account(mut, close = receiver)]`：`close()`在instructions执行的最后被调用，然后将`discriminator`设置为了`CLOSED_ACCOUNT_DISCRIMINATOR`。这是一个特殊的值，它会组织账户被重新初始化

