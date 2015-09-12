# �i�r�Q�[�V�������̋���

Prism��Region���g�����i�r�Q�[�V�����ɂ��ďڍׂɐ������܂��B
Prism�̃i�r�Q�[�V�����́A�f�t�H���g�ł͈�x�ǉ����ꂽ��ʂ��ė��p����悤�ɓ��삵�܂��B
����́A���A������B�ɑJ�ڂ��āA���A�ɖ߂��Ă����Ƃ��ɁA�O�̉��A�����̂܂܎g���邱�Ƃ��Ӗ����܂��B

���̂��߁A��ʑJ�ڎ��ɂ�����ƃf�[�^�̏������Ȃǂ��s���K�v������܂��B
��ʑJ�ڂ̏������t�b�N������@�Ƃ��āAPrism�ł́APrism.Regions.INavigationAware�C���^�[�t�F�[�X��񋟂��Ă��܂��B
���̃C���^�[�t�F�[�X�͈ȉ��̃��\�b�h���`���Ă��܂��B

- bool IsNavigationTarget(NavigationContext)
	- �����œn���ꂽ�R���e�L�X�g�̃^�[�Q�b�g�ƂȂ��ʂ��ǂ�����Ԃ��܂��B
- void OnNavigatedFrom(NavigationContext)
	- ��ʂ��痣���Ƃ��ɌĂяo����܂��B
- void OnNavigatedTo(NavigationContext)
	- ��ʂɑJ�ڂ��Ă����Ƃ��ɌĂяo����܂��B

�܂�AOnNavigatedTo�ŏ���������������Ηǂ����ƂɂȂ�܂��B

## �V�����C���X�^���X����肽��

�ꍇ�ɂ���Ă͓����C���X�^���X���g���܂킳�ꂽ���Ȃ��P�[�X������܂��B
�Ⴆ�΁ATabControl��Region�ɂ��Ă���Ƃ��ɁA����View�����ǈႤ�R���e���c��\���������Ƃ������P�[�X���l�����܂��B
���̂Ƃ��ɂ́AIsNavigationTarget���\�b�h�����ɗ����܂��B

���̃��\�b�h��true��Ԃ��ƁA����View���ė��p�����d�g�݂ɂȂ��Ă��܂��B�iViewModel��INavigationAware���������ĂȂ��Ƃ��͏��true�Ƃ݂Ȃ���Ă�̂ōė��p�����j
���̃��\�b�h�ŁANavigationContext�̃p�����[�^�Ȃǂ����ēK�؂�true��false��Ԃ����Ƃŉ�ʑJ�ڎ��ɐV����View����邩���Ȃ�������ł��܂��B

NavigationSampleApp�ł́ANavigation�̃p�����[�^���g�����Ƃ�AView�Ƃ���View��2�C���X�^���X�����ĊǗ����Ă��܂��B
AViewModel�̃R�[�h���ȉ��Ɏ����܂��B

```cs
using Prism.Mvvm;
using Prism.Regions;
using System.Diagnostics;

namespace NavigationSampleApp.ViewModels
{
    class AViewModel : BindableBase, INavigationAware
    {
        private string id;

        public string Id
        {
            get { return this.id; }
            set { this.SetProperty(ref this.id, value); }
        }


        public bool IsNavigationTarget(NavigationContext navigationContext)
        {
            return this.Id == navigationContext.Parameters["id"] as string;
        }

        public void OnNavigatedFrom(NavigationContext navigationContext)
        {
            Debug.WriteLine("NavigatedFrom");
        }

        public void OnNavigatedTo(NavigationContext navigationContext)
        {
            Debug.WriteLine("NavigatedTo");
            this.Id = navigationContext.Parameters["id"] as string;
        }
    }
}
```

�i�r�Q�[�V�������鑤�ł́A�ȉ��̂悤�Ƀp�����[�^��n���ĉ�ʑJ�ڂ���悤�ɂ��Ă��܂��B

```cs
this.RegionManager.RequestNavigate("MainRegion", nameof(AView), new NavigationParameters($"id={x}"));
```

���̂悤�ɂ��邱�ƂŁA�ׂ���View�̐��䂪�o���܂��B

## View��j������

��x�������ꂽView��j������ɂ́APrism.Regions.IRegionMemberLifetime�C���^�[�t�F�[�X���������܂��B
���̃C���^�[�t�F�[�X�ɒ�`����Ă���KeepAlive��false�ɂ��邱�ƂŁARegion�̃A�N�e�B�u��View���ς��^�C�~���O��
View�̔j�����s���܂��B

����������ł����A�{�^���������ꂽ�iCommand�����s���ꂽ�j�玩�����g��ViewModel����E�����@���Љ�܂��B
KeepAlive��false�ɐݒ肵�āA�����̂���Region���玩�����g��View��T���Ĕ�A�N�e�B�u�ɂ��Ă��܂��B

```cs
using Microsoft.Practices.Unity;
using Prism.Commands;
using Prism.Common;
using Prism.Regions;
using System.Diagnostics;
using System.Linq;

namespace NavigationSampleApp.ViewModels
{
    class BViewModel : IRegionMemberLifetime, INavigationAware
    {
        public DelegateCommand CloseCommand { get; }

        [Dependency]
        public IRegionManager RegionManager { get; set; }

        public bool KeepAlive { get; set; } = true;

        public BViewModel()
        {
            this.CloseCommand = new DelegateCommand(() =>
            {
                this.KeepAlive = false;
                // find view by region
                var view = this.RegionManager.Regions["MainRegion"]
                    .ActiveViews
                    .Where(x => MvvmHelpers.GetImplementerFromViewOrViewModel<BViewModel>(x) == this)
                    .First();
                // deactive view
                this.RegionManager.Regions["MainRegion"].Deactivate(view);
            });
        }

        public void OnNavigatedTo(NavigationContext navigationContext)
        {
            Debug.WriteLine("BViewModel NavigatedTo");
        }

        public bool IsNavigationTarget(NavigationContext navigationContext)
        {
            return true;
        }

        public void OnNavigatedFrom(NavigationContext navigationContext)
        {
            Debug.WriteLine("BViewModel NavigatedFrom");
        }
    }
}
```

�����܂ŏ�����View�����ڂƂ��Ȃ�Region����f����Remove���Ă������C�����Ă��܂����B����̃P�[�X�ł͂��D�݂̕��@�łǂ����B
�ʉ�ʂɑJ�ڂ���O�ɁA���̉�ʂ͔j���������Ƃ��Ȃǂ́A����KeepAlive���g�����@���������߂ł��B