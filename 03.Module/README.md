# Module��Region

Prism�ɂ́AModule�ƌĂ΂��@�\������܂��B����̓A�v���P�[�V�����𕡐��̋@�\�ɕ������ĊJ�����邽�߂̎d�g�݂ł��BPrism�ł͍ŏI�I��Module�ɕ��������A�v���P�[�V�������܂Ƃߏグ��@�\������܂��B

## Module�̎g����

�����ł͊ȒP��Module�̎g�����������܂��B�܂��AModule�����ɂ̓N���X���C�u�����v���W�F�N�g���쐬���܂��BWPF�̃N���X�Q���ŏ�����g����悤�ɃJ�X�^���R���g���[�������[�U�R���g���[���p�̃��C�u�������쐬����̂���Ԃ����Ȃ��Ă����ł��B�N���X���C�u�������쐬������A������Ԃō쐬����Ă���N���X���폜����Prism.Core��Prism.Unity��NuGet����ǉ����܂��B
�����āAPrism.Modularity.IModule�C���^�[�t�F�[�X�����������N���X���쐬���܂��B���̃N���X��Prism�̃��W���[���̃G���g���|�C���g�ɂȂ�܂��B

�쐬����Module�́ABootstrapper�N���X�̂���v���W�F�N�g�Ƀv���W�F�N�g�Q�Ƃ�ǉ����āAConfigureModuleCatalog���\�b�h���I�[�o�[���C�h���Ĉȉ��̂悤�ȃR�[�h��Prism��Module�����邱�Ƃ�`���܂��B

```cs
protected override void ConfigureModuleCatalog()
{
    base.ConfigureModuleCatalog();

    var catalog = (ModuleCatalog)this.ModuleCatalog;
    catalog.AddModule(typeof(HelloWorldModule.HelloWorldModule));
}
```

�����ł́AHelloWorldModule.HelloWorldModule�N���X��IModule�����������N���X�ɂȂ�܂��B

### IModule�C���^�[�t�F�[�X

IModule�C���^�[�t�F�[�X�́AInitialize���\�b�h���������̃V���v���ȃC���^�[�t�F�[�X�ł��B������Module�̏������������s���܂��B

## Region�ɂ���

Module���쐬������̓I�ȃR�[�h�̑O��Region�ɂ��ĐG��Ă��������Ǝv���܂��BPrism��Module��g�ݗ��Ă邱�Ƃŏ_��ɃA�v���P�[�V�������\�z�ł��܂��B
Module�Ԃ͑a�����ɍ����̂����z�I�ŁAModule���̃N���X�́A��������̃��b�Z�[�W���O�@�\���g���ĘA�g����̂����z�I�ł��B�iPrism�ɂ́A���b�Z�[�W���O�@�\���p�ӂ���Ă��܂��j

�_���Module��g�ݍ��킹�ăA�v���P�[�V��������邽�߂̉�ʑ��̎d�g�݂Ƃ���Region�Ƃ������̂�����܂��B��́AShell���������̋��iRegion�j�ɂ킯�āA�����ɑ΂��ĉ�ʕ��i�𗬂����ނ��Ƃŉ�ʑ�������Module�ō\�����ꂽ�Ƃ��ɂ��_��ɑΉ��ł���悤�ɂ��Ă��܂��B

Region���g���ɂ́A��ʂ̋��Ƃ��Ĉ��������Ƃ����RegionName��t���܂��B

```xml
xmlns:prism="http://prismlibrary.com/"

<ContentControl prism:RegionManager.RegionName="MainRegion" />
```

RegionName�������R���g���[���́AContentControl�̂ق���ItemsControl(���p�������R���g���[��)������܂��B
ContentControl�́ARegion���ŃA�N�e�B�u�ɂȂ��View��1�x��1�Ȃ̂ɑ΂��āAItemsControl�͕�����View�ɑΉ����Ă���_���قȂ�܂��B

���̂悤��Region���쐬������APrism�̒񋟂���IRegionManager��RequestNavigate���Ăяo�����Ƃ�View��\���ł��܂��B


```cs
this.RegionManager.RequestNavigate("MainRegion", nameof(HelloWorldView));
```

## Module��Region���g�����v���O������

�Ƃ������ƂŁAModule��Region���g�����ȒP�ȃv���O������g��ł��������Ǝv���܂��BModuleApp�Ƃ������O��WPF�A�v���P�[�V�������쐬���āAPrism.Core��Prism.Unity��NuGet����ǉ����܂��B
�����āAShell.xaml���쐬����Bootstrapper�N���X���쐬���āA�\������Ƃ���܂ō쐬���܂��B

### Module�̍쐬

ModuleApp.HelloWorldModule�Ƃ������O��WPF �J�X�^���R���g���[�� ���C�u�������쐬���āA�J�X�^���R���g���[���̃R�[�h���폜���܂��B������Prism.Core��Prism.Unity��NuGet����ǉ����܂��B
�����Ɋe��N���X������Ă����܂��B

#### Models, ViewModels, Views���O���

����ɂ��͐��E�ƕ\������View���쐬���܂��B�܂��AModels���O��ԂɈȉ��̂悤�ȃ��b�Z�[�W��񋟂���N���X���쐬���܂��B

```cs
namespace ModuleApp.HelloWorldModule.Models
{
    class MessageProvider
    {
        public string Message { get; } = "����ɂ��͐��E";
    }
}
```

�����āAHelloWorldViewModel�N���X���쐬���܂��B��قǂ�MessageProvider���C���W�F�N�V��������悤�ɂ��Ă��܂��B

```cs
using Microsoft.Practices.Unity;
using ModuleApp.HelloWorldModule.Models;

namespace ModuleApp.HelloWorldModule.ViewModels
{
    class HelloWorldViewModel
    {
        [Dependency]
        public MessageProvider MessageProvider { get; set; }
    }
}
```

�Ō�ɁAHelloWorldView���쐬���܂��BViews����ԂɃ��[�U�[�R���g���[�����쐬���āAXAML���ȉ��̂悤�ɕҏW���܂��B

```xml
<UserControl x:Class="ModuleApp.HelloWorldModule.Views.HelloWorldView"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:ModuleApp.HelloWorldModule.Views"
             xmlns:prism="http://prismlibrary.com/"
             prism:ViewModelLocator.AutoWireViewModel="True"
             mc:Ignorable="d" 
             d:DesignHeight="300" d:DesignWidth="300">
    <Grid>
        <TextBlock Text="{Binding MessageProvider.Message}" />
    </Grid>
</UserControl>
```

prism���O��Ԃ̒ǉ���ViewModelLocator�̒ǉ������āA��قǍ쐬����HelloWorldViewModel��DataContext�ɐݒ肳���悤�ɂ��Ă��܂��B
�����āATextBlock��MessageProvider��Message��\������悤�ɂ��Ă��܂��B

#### IModule�C���^�[�t�F�[�X�̎���

IModule�C���^�[�t�F�[�X�����������N���X���쐬���܂��BHelloWorldModule�Ƃ������O�̃N���X���v���W�F�N�g�����ɍ쐬���܂��B
HelloWorldModule�N���X��Initialize���\�b�h��Module�Ŏg�p����N���X��IUnityContainer�ɓo�^������A��ʂ�Region�ɕ\��������Ƃ������������s���܂��B

```cs
using Microsoft.Practices.Unity;
using ModuleApp.HelloWorldModule.Models;
using ModuleApp.HelloWorldModule.Views;
using Prism.Modularity;
using Prism.Regions;

namespace ModuleApp.HelloWorldModule
{
    public class HelloWorldModule : IModule
    {
        [Dependency]
        public IUnityContainer Container { get; set; }

        [Dependency]
        public IRegionManager RegionManager { get; set; }

        public void Initialize()
        {
            this.Container.RegisterType<MessageProvider>(new ContainerControlledLifetimeManager());
            this.Container.RegisterType<object, HelloWorldView>(nameof(HelloWorldView));

            this.RegionManager.RequestNavigate("MainRegion", nameof(HelloWorldView));
        }
    }
}
```

IUnityContainer��IRegionManager���APrism����C���W�F�N�V�������Ă��炤���߂�Dependency�����������v���p�e�B�Ƃ��Ē�`���Ă��܂��B
�����āAInitialize���\�b�h��RegisterType�����ĕK�v�ȃN���X��o�^���Ă��܂��B

���ӓ_�Ƃ��ẮAView�̓o�^�͕K��object�^�Ƃ��ēo�^����K�v������_�ł��B�܂����O�͌^���œo�^����̂���ʓI�ł��B

�Ō��RegionManager��RequestNavigate���\�b�h�ŁAShell�ɒ�`���ꂽMainRegion��HelloWorldView�i���Unity�ɓo�^���Ă����j�𗬂�����ł��܂��B

�����܂łŁA�v���W�F�N�g�͑�̂���Ȍ`�ɂȂ��Ă��܂��B

- ModuleApp
	- Views
		- Shell.xaml
	- Bootstrapper.cs
- ModuleApp.HelloWorldModule
	- Models
		- MessageProvider.cs
	- ViewModels
		- HelloWorldViewModel.cs
	- Views
		- HelloWorldView.xaml
	- HelloWorldModule.cs

#### Module��Bootstrapper�őg�ݍ���

�Ō�ɁAModule��g�ݍ��ރR�[�h��Bootstrapper�ɒǉ����܂��BModule�̍\���́ABootstrapper��ConfigureModuleCatalog���\�b�h�ōs���܂��B
������ModuleCatalog(IModuleCatalog�^)��ModuleCatalog�^�ɃL���X�g���āAAddModule�Ő�قǍ쐬����HelloWorldModule��ǉ����܂��B

```cs
using Microsoft.Practices.Unity;
using ModuleApp.Views;
using Prism.Modularity;
using Prism.Unity;
using System.Windows;

namespace ModuleApp
{
    class Bootstrapper : UnityBootstrapper
    {
        protected override DependencyObject CreateShell()
        {
            return this.Container.Resolve<Shell>();
        }

        protected override void InitializeShell()
        {
            ((Window)this.Shell).Show();
        }

        protected override void ConfigureModuleCatalog()
        {
            base.ConfigureModuleCatalog();

            var catalog = (ModuleCatalog)this.ModuleCatalog;
            catalog.AddModule(typeof(HelloWorldModule.HelloWorldModule));
        }

    }
}
```

���s����ƁAModule��Shell��HelloWorldModule�Œ�`���ꂽHelloWorldView���\������āA��ʂɂ���ɂ��͐��E�ƕ\������邱�Ƃ��m�F�ł��܂��B

