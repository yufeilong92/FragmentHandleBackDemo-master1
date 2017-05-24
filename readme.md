# fragment 处理返回键
** 定义接口
public interface BackHandledInterface {
  
  	public abstract void setSelectedFragment(BackHandledFragment selectedFragment);
  }
 **创建fragment
  public abstract class BackHandledFragment extends Fragment {
  
  	protected BackHandledInterface mBackHandledInterface;
  
  	protected abstract boolean onBackPressed();
  	
  	@Override
  	public void onCreate(Bundle savedInstanceState) {
  		super.onCreate(savedInstanceState);
  		if(!(getActivity() instanceof BackHandledInterface)){
  			throw new ClassCastException("Hosting Activity must implement BackHandledInterface");
  		}else{
  			this.mBackHandledInterface = (BackHandledInterface)getActivity();
  		}
  	}
  	
  	@Override
  	public void onStart() {
  		super.onStart();
  
  		mBackHandledInterface.setSelectedFragment(this);
  	}
  	
  }
  **使用的fragment 集成与BackHandledFragment重写方法
  
  	protected boolean onBackPressed() {
  		if(hadIntercept){
  			return false;
  		}else{
  			Toast.makeText(getActivity(), "Click From MyFragment", Toast.LENGTH_SHORT).show();
  			hadIntercept = true;
  			return true;
  		}
  	}
  **在现在的activity 中重写方法
  @Override
  	public void setSelectedFragment(BackHandledFragment selectedFragment) {
  		this.mBackHandedFragment = selectedFragment;
  	}
  	
  	@Override
  	public void onBackPressed() {
  		if(mBackHandedFragment == null || !mBackHandedFragment.onBackPressed()){
  			if(getSupportFragmentManager().getBackStackEntryCount() == 0){
  				super.onBackPressed();
  			}else{
  				getSupportFragmentManager().popBackStack();
  			}
  		}
  	}
