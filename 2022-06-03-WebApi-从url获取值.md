# 从url获取值

## 更新和删除时的 ID 的获取

``` cs
// 获取URL 中的 ID
[HttpPut]
[Route("{id}")]
public Users Update(Guid id, UserCto model)
{
    var tmp = _userRes.GetById(id);
    if (tmp != null)
    {
        tmp.UserName = model.UserName;
        tmp.PassWord = model.PassWord;
        tmp.IsActived = model.IsActived;
        tmp.IsDeleted = model.IsDeleted;

        _userRes.Update (tmp);
    }
    return tmp;
}
```

或者在需要从url 中获取的数据前加上 [FromQuery]

``` cs
[HttpPut]
[Route("{id}")]
public Users Update([FromQuery] Guid id, UserCto model)
{
    var tmp = _userRes.GetById(id);
    if (tmp != null)
    {
        tmp.UserName = model.UserName;
        tmp.PassWord = model.PassWord;
        tmp.IsActived = model.IsActived;
        tmp.IsDeleted = model.IsDeleted;

        _userRes.Update (tmp);
    }
    return tmp;
}
```