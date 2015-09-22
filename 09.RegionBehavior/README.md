# RegionBehavior

Prism��Region�́AWPF��Behavior�݂����ȋ@�\��񋟂���RegionBehavior�Ƃ������̂�񋟂��Ă��܂��B
���܂莩���ō�邱�Ƃ͂Ȃ��Ǝv���܂����ARegion����폜���ꂽ�Ƃ���IDisposable����������V��VM�ɑ΂��Ă�Dispose���ĂԂƂ����͎̂��v���肻�����Ǝv�����̂ŏ����Ă݂܂��B

## RegionBehavior

RegionBehavior�͒P���ŁARegion�v���p�e�B��Attach���\�b�h�������������̃V���v���ȃC���^�[�t�F�[�X���������邾���ł��B
Attach�ŁARegion�̃C�x���g���w�ǂ��Ă��ɂ傲�ɂ傷�邾���ł��BDispose���Ă�Behavior�͂���Ȋ����ɂȂ�܂��B

```cs
using Prism.Common;
using Prism.Regions;
using System;
using System.Collections.Specialized;

namespace DisposeBehaviorSampleApp.RegionBehaviors
{
    class DisposeBehavior : IRegionBehavior
    {
        public const string Key = nameof(DisposeBehavior);

        public IRegion Region { get; set; }

        public void Attach()
        {
            this.Region.Views.CollectionChanged += this.Views_CollectionChanged;
        }

        private void Views_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
        {
            if (e.Action == NotifyCollectionChangedAction.Remove)
            {
                Action<IDisposable> callDispose = d => d.Dispose();
                foreach (var o in e.OldItems)
                {
                    MvvmHelpers.ViewAndViewModelAction(o, callDispose);
                }
            }
        }
    }
}
```

Region�̂��ׂĂ�View���i�[���Ă���Views�R���N�V�����̕ύX���w�ǂ��Ă��܂��B
Views��ViewsCollection�Ƃ����^�ŁACollectionChanged��Add��Remove������΂��Ȃ��̂łǂ��炩����������΂����ł��B
����͍폜����Dispose���ĂԂƂ��������Ȃ̂�Remove����������΂������ƂɂȂ�܂��B

Prism�ɂ�MvvmHelpers�Ƃ����N���X�������āAV��VM�ɑ΂��āA���\�b�h���Ăׂ���ĂԂƂ����w���p�[������܂��B
�����������č폜���ꂽView�ɑ΂���Dispose���Ăяo���Ă��܂��B

�����RegionBehavior�́ABootstrapper��ConfigureDefaultRegionBehaviors���\�b�h�œo�^���܂��B�ȉ��̂悤�Ȋ����ł��B

```cs
using DisposeBehaviorSampleApp.RegionBehaviors;
using Microsoft.Practices.Unity;
using Prism.Modularity;
using Prism.Regions;
using Prism.Unity;
using System.Windows;

namespace DisposeBehaviorSampleApp
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
            var c = (ModuleCatalog)this.ModuleCatalog;
            c.AddModule(typeof(ModuleA.Module));
        }

        protected override IRegionBehaviorFactory ConfigureDefaultRegionBehaviors()
        {
            var f = base.ConfigureDefaultRegionBehaviors();
            f.AddIfMissing(DisposeBehavior.Key, typeof(DisposeBehavior));
            return f;
        }
    }
}
```

�������邱�ƂŁARegion�ɑ΂���RegionBehavior�������I�ɒǉ������悤�ɂȂ�܂��B�f�t�H���g�ł���������RegionBehavior���o�^����Ă�̂ŋ����̂������Prism�̃\�[�X�����Ă݂�Ƃ����Ǝv���܂��B
�ȉ��̂悤��IDisposable���������āA����View�i��ViewModel�j��Region����Remove����Ǝ����I��Dispose���Ă΂�邱�Ƃ��m�F�ł��܂��B�T���v���ł̓f�o�b�O�E�B���h�E�Ƀ��b�Z�[�W���o���Ă��܂��B

```cs
using Microsoft.Practices.Unity;
using Prism.Regions;
using System;
using System.Diagnostics;
using System.Windows;
using System.Windows.Controls;

namespace DisposeBehaviorSampleApp.ModuleA.Views
{
    /// <summary>
    /// ContentView.xaml �̑��ݍ�p���W�b�N
    /// </summary>
    public partial class ContentView : UserControl, INavigationAware, IDisposable
    {
        [Dependency]
        public IRegionManager RegionManager { get; set; }

        public ContentView()
        {
            InitializeComponent();
        }

        public void Dispose()
        {
            Debug.WriteLine("ContentView#Dispose");
        }

        public bool IsNavigationTarget(NavigationContext navigationContext)
        {
            return false;
        }

        public void OnNavigatedFrom(NavigationContext navigationContext)
        {
        }

        public void OnNavigatedTo(NavigationContext navigationContext)
        {
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            this.RegionManager.Regions["MainRegion"].Remove(this);
        }
    }
}
```


