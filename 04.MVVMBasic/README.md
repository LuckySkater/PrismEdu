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

## ErrorsContainer�N���X(WPF�̂�)

Prism�ɂ́AINotifyDataErrorInfo�C���^�[�t�F�[�X�̎�����⏕����ErrorsContainer�N���X������܂��B���̃N���X���g�����ƂŊȒP��INotifyDataErrorInfo���g�������͒l�̌��؋@�\���������N���X���쐬�ł��܂��B
ErrorsContainer�N���X�́A�^�����ɃG���[�̌^(���string)���w�肵�Ďg���܂��B
�R���X�g���N�^�ɂ́AErrorsChanged�C�x���g�̌Ăяo��������n���܂��B
�����āAINotifyDataErrorInfo�C���^�[�t�F�[�X��HasErrs�v���p�e�B�ƁAGetErrors���\�b�h�̏���������Ă���郁�\�b�h�������Ă��܂��B���̂��߁AINotifyDataErrorInfo�̎����N���X�ł�
ErrorsContainer�Ɉڏ����邾���ł��݂܂��B

���Ƃ́A�C�ӂ̃^�C�~���O��SetErrors(propertyName, errorInfo)�ŃG���[����ݒ肵�āAClearErrors(propertyName)�ŃG���[�̃N���A�����邾���ł��B�ȉ��ɊȒP��INotifyDataErrorInfo�����������N���X�̗�������܂��B

```cs
using Prism.Mvvm;
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;
using System.Linq;
using System.Runtime.CompilerServices;

namespace MVVMBasicApp.Module.ViewModels
{
    class ErrorsContainerSampleViewModel : BindableBase, INotifyDataErrorInfo
    {
        public string HeaderText { get; } = "ErrorContainerSample";

        private string input;

        [Required(ErrorMessage = "���͂��Ă�������")]
        public string Input
        {
            get { return this.input; }
            set { this.SetProperty(ref this.input, value); }
        }


        public ErrorsContainerSampleViewModel()
        {
            this.ErrorsContainer = new ErrorsContainer<string>(
                x => this.ErrorsChanged?.Invoke(this, new DataErrorsChangedEventArgs(x)));
        }

        #region Validation
        private ErrorsContainer<string> ErrorsContainer { get; }

        public event EventHandler<DataErrorsChangedEventArgs> ErrorsChanged;

        protected override bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
        {
            if(!base.SetProperty<T>(ref storage, value, propertyName))
            {
                return false;
            }

            var context = new ValidationContext(this)
            {
                MemberName = propertyName
            };

            var errors = new List<ValidationResult>();
            if (!Validator.TryValidateProperty(value, context, errors))
            {
                this.ErrorsContainer.SetErrors(propertyName, errors.Select(x => x.ErrorMessage));
            }
            else
            {
                this.ErrorsContainer.ClearErrors(propertyName);
            }

            return true;
        }

        public bool HasErrors
        {
            get
            {
                return this.ErrorsContainer.HasErrors;
            }
        }
		Data
        public IEnumerable GetErrors(string propertyName)
        {
            return this.ErrorsContainer.GetErrors(propertyName);
        }
        #endregion
    }
}
```

BindableBase�N���X��SetProperty���\�b�h���I�[�o�[���C�h���āADataAnnotations���g�������͒l�̌��؂����Ă���N���X�ɂȂ�܂��BInput�v���p�e�B�������͂̏ꍇ�̓G���[�ɂȂ�܂��B
