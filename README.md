#include <iostream>
#include <cstdlib>
#include <ctime>
#include <locale.h>
 
namespace cf {
    
    class Object{
        protected:
            int var;
        public:
            virtual void turn() = 0;    
            virtual int answer() const { return var; }
            virtual void reset() { var = 0; }
            virtual bool compare(const Object * o) const  { return var == o->answer(); }
    };
    
    class User : public Object{
        public:
            User(){
                var = 0;
            }
            virtual ~User(){}
            virtual void turn(){
                std::cout << "Ïîëüçîâàòåëü âûáèðàåò îò 1 - 10: ";
                std::cin >> var;
            }   
    };
    
    class AI : public Object{
        public:
            AI(){
                var = 0;
            }
            virtual ~AI(){}
            virtual void turn(){
                std::cout << "ÈÈ âûáèðàåò îò 1 - 10: ";
                var = rand() % 10 + 1;
            }   
    };
}
 
int main(int argc, char** argv) {
		setlocale(LC_CTYPE,"Russian");
    
    bool turn = true;
    char n;
    cf::Object * user = new cf::User();
    cf::Object * ai = new cf::AI();
    
    std::cout << "Ïîèãðàåì ? (y-äà, q-íåò) ";
    while(std::cin >> n) // for exit input 'q' or 'Q'
    {
        if(n == 'q' || n == 'Q') 
        {
            std::cout << "çà÷åì çàïóñòèë, åñëè íå èãðàåøü?." << std::endl;
            break;
        }
        
        if(turn) // turn user
        {
            std::cout << "Âûáèðàåò ïîëüçîâàòåëü" << std::endl;
            ai->turn(); std::cout << std::endl;
            user->turn(); std::cout << std::endl;
            if(ai->compare(user))
                std::cout << "ïîáåäà" << std::endl;
            else
                std::cout << "ïîëüçîâàòåëü íå óãàäàë " << ai->answer() << std::endl; 
            
            turn = !turn;
        }
        else // turn ai
        {
            std::cout << "âûáèðàåò èè" << std::endl;
            user->turn(); std::cout << std::endl;
            ai->turn(); std::cout << ai->answer() << std::endl;
            if(user->compare(ai))
                std::cout << "èè âûèãðàë" << std::endl;
            else
                std::cout << "èè îøèáñÿ" << std::endl; 
                
            turn = !turn;
        }
        
        std::cout << "ñûãðàòü åù¸? ??" <<std::endl;
    }
    
    return 0;
}
