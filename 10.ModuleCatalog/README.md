# ModuleCatalog�̍\��

ModuleCatalog�́A����܂�ModuleCatalog��API���g���č\�����Ă��܂����B
�����ł́A����ɉ�����App.config�Ō������s�����@�ɂ��ďЉ�܂��B

## App.config�ɂ��\��

App.config�ɂ��ModuleCatalog���\������ɂ́A�ȉ��̂悤��App.cinfig�t�@�C����ҏW���܂��B

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <section name="modules" type="Prism.Modularity.ModulesConfigurationSection, Prism.Wpf" />
  </configSections>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
  <modules>
    <module assemblyFile="ModuleA.dll" moduleType="ModuleA.ModuleAModule, ModuleA, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" moduleName="ModuleAModule" startupLoaded="True" />
    <module assemblyFile="ModuleB.dll" moduleType="ModuleB.ModuleBModule, ModuleB, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" moduleName="ModuleBModule" startupLoaded="True">
      <dependencies>
        <dependency moduleName="ModuleAModule" />
      </dependencies>
    </module>
    <module assemblyFile="ModuleC.dll" moduleType="ModuleC.ModuleCModule, ModuleC, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" moduleName="ModuleCModule" startupLoaded="False" />
  </modules>
</configuration>
```

section�^�O��modules�^�O���`���Ă��܂��B
modules�^�O�ɂ�module�^�O���`�ł��āA���̃^�O��assemblyFile�Ń��W���[���̂���A�Z���u�����AmoduleType�Ń��W���[���̌^���AmoduleName�Ń��W���[���̖��O�AstartupLoaded�ŋN�����Ƀ��W���[����ǂݍ��ނ��ǂ����w�肵�܂��B

���W���[�����ʃ��W���[���Ɉˑ����Ă���ꍇ��module�^�O����dependencies�^�O�������āA���̒���dependency�^�O��moduleName���g���Ĉˑ����郂�W���[�����`���܂��B
��L��ł́AModuleBModule��ModuleAModule�Ɉˑ����Ă���悤�ɂ��Ă��܂��B

## App.config��ǂݍ��ނ悤�ɂ���

�f�t�H���g�ł́APrism.Wpf��Bootstrapper��App.config����\����ǂݍ��݂܂���B
���̓����ς���ɂ�Bootstrapper�N���X��CreateModuleCatalog���\�b�h���I�[�o�[���C�h����ConfigurationModuleCatalog��Ԃ��悤�ɂ��܂��B
�������邱�ƂŁAApp.config�ɒ�`�������W���[���̍\����ǂݍ��ނ悤�ɂȂ�܂��B

```cs
using Microsoft.Practices.Unity;
using Prism.Unity;
using ConfigureModuleCatalogApp.Views;
using System.Windows;
using Prism.Modularity;

namespace ConfigureModuleCatalogApp
{
    class Bootstrapper : UnityBootstrapper
    {
        protected override IModuleCatalog CreateModuleCatalog()
        {
            return new ConfigurationModuleCatalog();
        }

        protected override DependencyObject CreateShell()
        {
            return Container.Resolve<MainWindow>();
        }

        protected override void InitializeShell()
        {
            Application.Current.MainWindow.Show();
        }
    }
}

```

