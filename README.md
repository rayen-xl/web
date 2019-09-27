# web
typedef struct _DVE_SN
{
   uint8_t vendor;
   uint8_t dev_type:5;
   uint8_t subcode:3;
   
   uint8_t year;
   uint8_t month;
   uint8_t day;
   uint8_t num_h;
   uint8_t num_l; 
   uint8_t check_sum;  
}DVE_SN;

uint8_t GetSnCheckSum(const DVE_SN * sn)
{
	  uint8_t * point=(uint8_t *)sn;
	  sn->check_sum=0;
    for(uint8_t i=0;i<sizeof(DVE_SN)-1;i++)
    {
        if(i==0)
        {
          sn->check_sum=(point[i]*point[i])-1;
        }
        else if(i==1)
        {
          sn->check_sum=(~point[i]);
        }
        else if(i==2)
        {
          sn->check_sum=(point[i])-point[6];
        }
        else if(i==5)
        {
          sn->check_sum=(~point[i])*point[i]+12;
        }
        else if(i==6)
        {
           sn->check_sum=(~point[i])*point[i]+48;
        }
        else
        {
          sn->check_sum=point[i];
        }
    }
}

void GenSn(const DVE_SN * sn)
{
   sn->check_sum= GetSnCheckSum(sn);
}

uint8_t CheckSnValid(uint8_t *sn)
{
	const DVE_SN * point=(DVE_SN *)sn;
    uint8_t ret=0;
    	if(GetSnCheckSum(point)!==point->check_sum)
	{
         ret=1;
	}
  }
	return ret;	
