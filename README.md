# cocos2dx+lua 学习

1.用cJSON  解析JSON

2.创建一个文字标签
    
   	CCLabelTTF* label = CCLabelTTF::create(title().c_str(), "Arial", 32);
   	addChild(label, 1);
   	label->setPosition( ccp(VisibleRect::center().x, VisibleRect::top().y-50) );        


3.创建一个spring

    
    m_pBall = CCSprite::create("Images/ball.png");
    m_pBall->setPosition(ccp(VisibleRect::center().x, VisibleRect::center().y));
    addChild(m_pBall);

    
4.获取舞台

   
    CCDirector* pDir = CCDirector::sharedDirector();
    
        
5.获取内容的大小和位置

    	CCSize ballSize  = m_pBall->getContentSize();
    	CCPoint ptNow  = m_pBall->getPosition();
    
6.切换场景

    CCDirector::sharedDirector()->replaceScene(this);    
    
7.陀螺仪调用

 	 setAccelerometerEnabled(true);
 	 void AccelerometerTest::didAccelerate(CCAcceleration* pAccelerationValue)

  
8.创建一组菜单
 
 
    CCMenuItemImage *item1 = CCMenuItemImage::create(s_pPathB1, s_pPathB2, this, menu_selector(ActionManagerTest::backCallback) );
    CCMenuItemImage *item2 = CCMenuItemImage::create(s_pPathR1, s_pPathR2, this, menu_selector(ActionManagerTest::restartCallback) );
    CCMenuItemImage *item3 = CCMenuItemImage::create(s_pPathF1, s_pPathF2, this, menu_selector(ActionManagerTest::nextCallback));

    CCMenu *menu = CCMenu::create(item1, item2, item3, NULL);

    menu->setPosition(CCPointZero);
    item1->setPosition(ccp(VisibleRect::center().x - item2->getContentSize().width*2, VisibleRect::bottom().y + item2->getContentSize().height/2));
    item2->setPosition(ccp(VisibleRect::center().x, VisibleRect::bottom().y + item2->getContentSize().height/2));
    item3->setPosition(ccp(VisibleRect::center().x + item2->getContentSize().width*2, VisibleRect::bottom().y + item2->getContentSize().height/2));
    
    addChild(menu, 1);   
    ```
9.
    









