# Module�̓ǂݍ��ݏ���

Prism��Module�͓ǂݍ��ݏ������w�肵����A�K�v�ɂȂ�܂œǂݍ��݂�x��������@�\������܂��B
�N�����ɁA�SModule��ǂݍ��ނƋN�����x���Ȃ�����A����Module��ǂݍ��ނ܂��ɓǂݍ���ł����Ȃ���΂Ȃ�Ȃ�Module�Ȃǂ�K�؂ɊǗ��ł��܂��B

�Ǘ��̎d���́A����������܂�����ԊȒP�ȕ��@��Module�̓o�^���ɍs�����@�ł��B

```cs
// ����Module��ǂݍ��ޑO�ɓǂݍ��܂Ȃ��Ƃ����Ȃ�Module���w�肵��Module��o�^����
AddModule(Type moduleType, params string[] dependsOn)

// ���������@���w�肵��Module��o�^����
AddModule(Type moduleType, InitializingMode initializingMode, params string[] dependsOn)
```

�Ⴆ�΃I���f�}���h�ɓǂݍ���BModule�ƁACommonModule�Ɉˑ�����AModule��o�^����ꍇ�͈ȉ��̂悤�ɂȂ�܂��B

```cs
protected override void ConfigureModuleCatalog()
{
    base.ConfigureModuleCatalog();

    var c = (ModuleCatalog)this.ModuleCatalog;
    // ondemand
    c.AddModule(typeof(BModule), InitializationMode.OnDemand);
    // depend CommonModule
    c.AddModule(typeof(AModule), nameof(CommonModule));
    c.AddModule(typeof(CommonModule));
}
```

���̂悤�ɂ���ƁA�A�v���P�[�V�����N������CommonModule �� AModule�̏��Ԃɏ���������܂��B
BModule�͋N�����ɂ͏���������܂���B

BModule��ǂݍ��ނɂ�IModuleManager���g�p���܂��B
IModuleManager��LoadModule�œǂݍ���Module�����w�肷�邱�Ƃœǂݍ��܂��邱�Ƃ��o���܂��B
BModule��ǂݍ��܂���R�[�h��͈ȉ��̂悤�ɂȂ�܂��B


```cs
[Dependency]
public IModuleManager ModuleManager { get; set; }

private void ButtonLoadModuleB_Click(object sender, RoutedEventArgs e)
{
    this.ModuleManager.LoadModule("BModule");
}
```

