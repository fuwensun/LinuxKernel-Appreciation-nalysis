# 1.3ģ�鶯̬����

�ܵ������ǣ�
	����׶Σ���������.ko�ļ���
	���н׶Σ�ͨ�� insmod ϵͳ���ã����ں˿ռ����xxx_mod_init()������

```
//sfw**module**module_init����(ģ�鶯̬����)
#define module_init(initfn)					\
//sfw**__inittest��������initfn������Ϊinitcall_t
	static inline initcall_t __maybe_unused __inittest(void)		\
	{ return initfn; }					\
//sfw**initfn������������Ϊinit_module
	int init_module(void) __attribute__((alias(#initfn)));	
```

