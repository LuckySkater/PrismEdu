# Prism.Windows�Ƃ�

Prism.Windows�́AUniversal Windows Platform�A�v���iUWP�A�v���j������Prism�ɂȂ�܂��B
UWP�A�v�����쐬���邽�߂ɁA�悭���悤�ȃN���X��������x��������Ă�����AXAML�n�v���b�g�t�H�[���ł͓S�̐݌v���@�ł���MVVM�p�^�[�����x������@�\���e�퐷�荞�܂�Ă��܂��B

Prism.Windows�𓱓����邱�Ƃœ����郁���b�g���ȉ��Ɏ����܂��B

- MVVM�̊�{�N���X
	- INotifyPropertyChanged�̎����N���X(BindableBase)
	- ICommand�̎����N���X(DelegateCommand)
	- ���؋@�\��INotifyPropertyChanged�̎����N���X(ValidatableBindableBase)
	- ��ʑJ�ڎ��̃R�[���o�b�N�ɑΉ�����ViewModel�̊�{�N���X(ViewModelBase)
	- �N���X�Ԃ̑a�����ȃ��b�Z�[�W�x�[�X�̘A�g�@�\�iIEventAggregator�j
- UWP�ŗL�̋@�\�ւ̑Ή�
	- �߂鏈���ւ̑Ή�
		- �^�C�g���o�[�ւ̖߂�{�^���̕\���E��\��
		- �߂�{�^���ւ̑Ή�
	- ���f�����ւ̑Ή�
		- ���f���Ɏw�肵��ViewModel�̃v���p�e�B��ۑ�����@�\
		- ViewModel, Model�Ŏg�p�\�Ȉꎞ�ۑ��f�[�^�p��ISessionStateService�C���^�[�t�F�[�X�̒�
		- ���f���ɉ�ʑJ�ڗ����̕ۑ��ƕ��A���ɉ�ʑJ�ڗ����̕���
	- ���O�x�[�X�̉�ʑJ�ڏ���
		- Views/xxxxPage�Ƃ��������K��ɏ]���Ă����xxxx�Ƃ���������̎w��ŉ�ʑJ�ڂ��\
			- �����K��̃J�X�^�}�C�Y�ɑΉ�
	- View��ViewModel�̎����R�Â��@�\
		- Views/xxxxPage�ɑ΂���ViewModels/xxxxPageViewModel�N���X�Ƃ��������K��Ŏ����I��DataContext��ViewModel��ݒ肷��@�\
			- �����K��̃J�X�^�}�C�Y�ɑΉ�
- DI�R���e�i�ւ̑Ή��iPrism.Unity.Windows�������j
	- Prism�̊e��T�[�r�X��񋟂���C���^�[�t�F�[�X�������I�ɃR���e�i�ɓo�^����@�\
		- ����ɂ��Model��ViewModel����V�[�����X��Prism�̋@�\��DI���Ďg�����Ƃ��ł���悤�ɂȂ�
	- DI�R���e�i�ɂ��ViewModel�̐����@�\
		- ����ɂ��ViewModel��DI���g�p�\�ɂȂ�

