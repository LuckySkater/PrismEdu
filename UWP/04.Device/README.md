# �߂�{�^���E�n�[�h�E�F�A�{�^���Ή�

UWP��Prism�ɂ́A�߂�{�^���ւ̑Ή���f�X�N�g�b�v�ł̃^�C�g���o�[�̖߂�{�^���̕\���E��\���Ή���A�d�b�̃n�[�h�E�F�A�̖߂�{�^���J�����{�^���ւ̑Ή��T�|�[�g�@�\������܂��B
���ԂɌ��Ă������Ǝv���܂��B

## DeviceGestureService
�f�o�C�X�֘A�̃T�[�r�X�ɃA�N�Z�X����ɂ́APrismUnityApplication�N���X�Œ�`����Ă���DeviceGestureService�v���p�e�B����IDeviceGestureService�C���^�[�t�F�[�X���擾���Ďg���܂��B
ViewModel�ŁA����DeviceGestureService���g���ɂ́AINavigationService�Ɠ��l��ViewModel�̃R���X�g���N�^��IDeviceGestureService���󂯎��悤�ɂ��܂��B

IDeviceGestureService�ɂ́A�e��n�[�h�E�F�A�̗L����₢���킹�邽�߂̈ȉ��̃v���p�e�B����`����Ă��܂��B

- IsHardwareCameraButtonPresent
- IsHardwareBackButtonPresent
- IsKeyboardPresent
- IsMousePresent
- IsTouchPresent

���O�̒ʂ�ォ�珇�ԂɁA�J�����{�^���A�߂�{�^���A�L�[�{�[�h�A�}�E�X�A�^�b�`���񋟂���Ă��邩�m�F�ł��܂��B

����ɁA�n�[�h�E�F�A�{�^���ƃ}�E�X�Ɋւ��Ă͊e�C�x���g���񋟂���Ă��܂��B

- GoBackRequested
- CameraButtonPressed
- CameraButtonHalfPressed
- CameraButtonReleased
- MouseMoved

���Ԃɖ߂鏈���A�J�����{�^���������ꂽ���A���������ꂽ���A�����ꂽ���A�}�E�X�����������Ƃ������C�x���g�����܂��B

## ViewModel�ł̎g�p

ViewModel��DeviceGestureService���g������ȉ��Ɏ����܂��B

```cs
using Prism.Windows.AppModel;
using Prism.Windows.Mvvm;
using Prism.Windows.Navigation;
using System.Collections.Generic;
using System.Diagnostics;

namespace DeviceApp.ViewModels
{
    class MainPageViewModel : ViewModelBase
    {
        private INavigationService NavigationService { get; }

        private IDeviceGestureService DeviceGestureService { get; }

        public bool HasCameraButton => this.DeviceGestureService.IsHardwareCameraButtonPresent;

        public bool HasBackButton => this.DeviceGestureService.IsHardwareBackButtonPresent;

        public bool HasKeyboard => this.DeviceGestureService.IsKeyboardPresent;

        public bool HasMouse => this.DeviceGestureService.IsMousePresent;

        public bool HasTouch => this.DeviceGestureService.IsTouchPresent;

        public MainPageViewModel(INavigationService ns, IDeviceGestureService ds)
        {
            this.NavigationService = ns;
            this.DeviceGestureService = ds;
        }

        public override void OnNavigatedTo(NavigatedToEventArgs e, Dictionary<string, object> viewModelState)
        {
            base.OnNavigatedTo(e, viewModelState);
            this.DeviceGestureService.GoBackRequested += this.DeviceGestureService_GoBackRequested;
            this.DeviceGestureService.CameraButtonPressed += this.DeviceGestureService_CameraButtonPressed;
            this.DeviceGestureService.MouseMoved += this.DeviceGestureService_MouseMoved;
        }

        private void DeviceGestureService_MouseMoved(object sender, Windows.Devices.Input.MouseEventArgs e)
        {
            Debug.WriteLine($"{e.MouseDelta.X}, {e.MouseDelta.Y}");
        }

        public void NavigateNextPage()
        {
            this.NavigationService.Navigate("Next", null);
        }

        private void DeviceGestureService_CameraButtonPressed(object sender, DeviceGestureEventArgs e)
        {
            Debug.WriteLine(nameof(this.DeviceGestureService_CameraButtonPressed));
        }

        private void DeviceGestureService_GoBackRequested(object sender, DeviceGestureEventArgs e)
        {
            Debug.WriteLine(nameof(this.DeviceGestureService_GoBackRequested));
            e.Handled = true;
            e.Cancel = true;
        }

        public override void OnNavigatingFrom(NavigatingFromEventArgs e, Dictionary<string, object> viewModelState, bool suspending)
        {
            base.OnNavigatingFrom(e, viewModelState, suspending);
            this.DeviceGestureService.GoBackRequested -= this.DeviceGestureService_GoBackRequested;
            this.DeviceGestureService.CameraButtonPressed -= this.DeviceGestureService_CameraButtonPressed;
            this.DeviceGestureService.MouseMoved -= this.DeviceGestureService_MouseMoved;
        }
    }
}
```

�e��v���p�e�B��ViewModel�̃v���p�e�B�Ƃ��Č��J���Ă���̂ƁA�߂�{�^���A�J�����{�^���A�}�E�X���������Ƃ��̃C�x���g���n���h�����O���Ă��܂��B
�߂�{�^���̃C�x���g�́AHandled��Cancel��True�ɐݒ肷�邱�ƂŁA�������L�����Z�����邱�Ƃ��ł��܂��BMainPage�ł�������Ɩ߂�{�^���ŏI���ł��Ȃ��A�v���Ȃǂ������ł��܂��B
�i������炢���Ȃ����낤���ǁj

����ViewModel�̃v���p�e�B���o�C���h������ʂ͈ȉ��̂悤�ɂȂ�܂��B

![1.png](Images/1.png)

![2.png](Images/2.png)

Mobile���G�~�����[�^�Ȃ������ATouch��False��Mouse��True�ɂȂ��Ă�̂��C�ɂȂ�܂��B���@���Ȃ��̂Ŏ��@�ł̓��삪�����Ȃ��ł��������ʓ����ł��ˁB

�܂��A�߂�{�^����J�����{�^���������ƃf�o�b�O�o�͂Ɉȉ��̂悤�ȃ��b�Z�[�W���\������܂��B

```
DeviceGestureService_GoBackRequested
DeviceGestureService_CameraButtonPressed
```

## �f�X�N�g�b�v�̃^�C�g���o�[�̖߂�{�^���̔�\��

Prism�ł̓f�t�H���g�Ńf�X�N�g�b�v�̖߂�{�^�����\������܂��B
������\���ɂ���ɂ́ADeviceGestureService�̍쐬�������ȉ��̂悤�ɃI�[�o�[���C�h����K�v������܂��B

```cs
// App.xaml.cs
protected override IDeviceGestureService OnCreateDeviceGestureService() => 
    new DeviceGestureService { UseTitleBarBackButton = false };
```

UseTitleBarBackButton��false��ݒ肵�Ă���_���|�C���g�ł��B



