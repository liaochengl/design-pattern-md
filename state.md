

状态模式 + 策略模式实现状态控制

定义状态接口

```java
public interface StateInterface {
    void switchState();//不同状态下具体实现各自业务
}
```

状态实现类若干(根据实际业务场景)

```java
public class State1 implements StateInterface {
    @Override
    public void switchState() {
        System.out.println("当前状态state1");
        System.out.println("切换至状态state2");
    }
}

public class State2 implements StateInterface {

    @Override
    public void switchState() {
        System.out.println("当前状态state2");
        System.out.println("切换至状态state3");
    }
}
```

状态属性宿主，这里对应业务场景中的诸如订单、活动等等

```java
public class StateContext {

    private StateInterface state;

    public StateContext(StateInterface state){
        this.state = state;
    }

    public void switchState(){
        state.switchState();
    }
}
```

初始化各状态容器

```java
public class Strategy {

    private static Map<String, StateInterface> map = new HashMap<>();

    static {
        map.put("1", new State1());
        map.put("2", new State2());
        map.put("3", new State3());
    }
    
    //提供获取状态方法
    public static StateInterface getStateInterface(String status){
        return map.get(status);
    }
}
```

业务调用

```java
public static void main(String[] args){
    String status = "1";//这里根据实际业务需要读取数据库，取到不同状态
    StateInterface state = Strategy.getStateInterface(status);
    StateContext context = new StateContext(state);
    context.switchState();
}
```

