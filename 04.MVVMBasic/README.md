# MVVM�̊�{�N���X

�����ł́APrism�Œ񋟂���Ă���MVVM�̊�{�N���X�ɂ��Đ������܂��B

## BindableBase�N���X

BindableBase�N���X�̓V���v����INotifyPropertyChanged�̎����N���X�ł��BSetProperty���\�b�h���g���āA�V���v���Ƀv���p�e�B���`�o���܂��B
�Ⴆ�΁AInput�Ƃ����v���p�e�B������AOutput�Ƃ���Input�����ׂđ啶���ɂ����v���p�e�B�����N���X�̒�`�͈ȉ��̂悤�ɂȂ�܂��B

```cs
using Prism.Mvvm;

namespace MVVMBasicApp.Module.ViewModels
{
    class BindableBaseSampleViewModel : BindableBase
    {
        public string HeaderText { get; } = "BindableBaseSample";

        private string input;

        public string Input
        {
            get { return this.input; }
            set
            {
                if (this.SetProperty(ref this.input, value))
                {
                    this.Output = this.Input?.ToUpper();
                }
            }
        }

        private string output;

        public string Output
        {
            get { return this.output; }
            private set { this.SetProperty(ref this.output, value); }
        }

    }
}
```

BindableBase�N���X�ł́A��L�̂悤�Ƀt�B�[���h���`���āAset���\�b�h�̒���SetProperty(ref, this.fieldName, value)�Ƃ����`�Œl�̐ݒ���s���܂��B
���ꂾ���ŁAPropertyChanged�C�x���g�܂Ŕ��s���Ă���܂��B

## DelegateCommand�N���X

MVVM�Ŏg�p����Command�̎����N���X��Prism�ł͒񋟂��Ă��܂��B
DelegateCommand�����̃N���X�ɂȂ�܂��B
DelegateCommand�́A�R���X�g���N�^�̈�����Execute���Ɏ��s����鏈���ƁACanExecute���Ɏ��s����鏈����n���܂��BCanExecute���̏����͏ȗ��\�ŁA�ȗ������ꍇ�͏�Ɏ��s�\�ȃR�}���h�ɂȂ�܂��B

Prism 6.0��DelegateCommand�̐V�@�\�Ƃ��āACanExecuteChanged�̌Ăяo�����v���p�e�B�̕ύX���Ď����ČĂяo���Ă����@�\���ǉ�����܂����B
ObserveProperty���\�b�h������ɂȂ�܂��B�ȉ��̂悤�Ɏg�p���܂��B

```cs
command.ObserveProperty(() => this.Hoge);
```

����ŁAHoge�v���p�e�B���Ď�����Hoge�v���p�e�B�ɕύX����������CanExecuteChanged���Ăяo���Ă���܂��B���̂ق��ɁAbool�^�̃v���p�e�B���󂯎��A���̒l��CanExecute�̌��ʂƂ��ĕԂ��v���p�e�B�̕ύX�Ď�������ObserveCanExecute���\�b�h���񋟂���Ă��܂��B

Command�����s������AInput�v���p�e�B�̒l��啶���ɂ���Output�v���p�e�B�ɕϊ�����R�}���h�iInput�������͂̏ꍇ�͎��s�ł��Ȃ��j��������ViewModel�͈ȉ��̂悤�ɂȂ�܂��B

```cs
using Prism.Commands;
using Prism.Mvvm;

namespace MVVMBasicApp.Module.ViewModels
{
    class DelegateCommandSampleViewModel : BindableBase
    {
        public string HeaderText { get; } = "DelegateCommandSample";

        private string input;

        public string Input
        {
            get { return this.input; }
            set { this.SetProperty(ref this.input, value); }
        }

        private string output;

        public string Output
        {
            get { return this.output; }
            set { this.SetProperty(ref this.output, value); }
        }

        public DelegateCommand ToUpperCommand { get; }

        public DelegateCommandSampleViewModel()
        {
            // create command
            this.ToUpperCommand = new DelegateCommand(() =>
                {
                    this.Output = this.Input.ToUpper();
                },
                () => !string.IsNullOrWhiteSpace(this.Input));

            // CanExecuteChanged trigger
            this.ToUpperCommand.ObservesProperty(() => this.Input);
        }
    }
}
```

