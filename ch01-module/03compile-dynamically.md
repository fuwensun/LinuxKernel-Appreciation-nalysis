# 1.3ģ�鶯̬����

�ܵ������ǣ�  
-    ����׶Σ���������.ko�ļ���  
-    ���н׶Σ�ͨ�� insmod ϵͳ���ã����ں˿ռ���� xxx_init()������  

## 1.3.1
������Ĵ����֪����̬�����ģ��ĳ�ʼ����������������Ϊinit_module��
```
//sfw**module**module_init����(ģ�鶯̬����)
#define module_init(initfn)					\
	static inline initcall_t __maybe_unused __inittest(void)\	//sfw**__inittest��������initfn������Ϊinitcall_t
	{ return initfn; }					\
	int init_module(void) __attribute__((alias(#initfn)));		//sfw**initfn������������Ϊinit_module
```

#1.3.2
��̬�����ģ��ͨ�� init_module ϵͳ���ã��������ں˿ռ䣬Ȼ��������ʼ���������ģ��ĳ�ʼ����
```
SYSCALL_DEFINE3(init_module, ...)
	load_module
		do_init_module(mod)
			do_one_initcall(mod->init)
```

�����ģ������֣�
-    ���ں˴�������ó���ģ��ķ�ʽ���룬ͨ�������ű��Զ����أ�ͨ��ϵͳ����init_module����
-    �û���д�ģ�ͨ���ű���ָ����أ�ͨ��ϵͳ����init_module����