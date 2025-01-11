<h1 align="center">👋 Welcome to My Portfolio</h1>

<div align="center">
  <p><i>Software Developer | Data Analyst | Full-Stack Developer</i></p>
</div>
import React, { useState, useEffect } from 'react';
import { Card } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Trophy, Heart } from 'lucide-react';

const MiniGame = () => {
  const [position, setPosition] = useState(0);
  const [obstacles, setObstacles] = useState([]);
  const [score, setScore] = useState(0);
  const [gameOver, setGameOver] = useState(false);
  const [lives, setLives] = useState(3);
  const [isPlaying, setIsPlaying] = useState(false);

  useEffect(() => {
    if (!isPlaying || gameOver) return;
    
    const gameLoop = setInterval(() => {
      setObstacles(prev => {
        // Move obstacles left
        const newObstacles = prev
          .map(obs => ({ ...obs, x: obs.x - 5 }))
          .filter(obs => obs.x > -20);

        // Check collisions
        newObstacles.forEach(obs => {
          if (Math.abs(obs.x - 50) < 20 && Math.abs(obs.y - position) < 30) {
            setLives(prev => {
              if (prev <= 1) {
                setGameOver(true);
                setIsPlaying(false);
              }
              return prev - 1;
            });
          }
        });

        return newObstacles;
      });

      setScore(prev => prev + 1);

      // Add new obstacles
      if (Math.random() < 0.03) {
        setObstacles(prev => [...prev, {
          x: 400,
          y: Math.floor(Math.random() * 280),
          id: Date.now()
        }]);
      }
    }, 50);

    return () => clearInterval(gameLoop);
  }, [isPlaying, position, gameOver]);

  const handleJump = () => {
    if (!isPlaying) return;
    setPosition(prev => Math.max(0, Math.min(280, prev + (prev > 140 ? -40 : 40))));
  };

  const startGame = () => {
    setScore(0);
    setLives(3);
    setPosition(140);
    setObstacles([]);
    setGameOver(false);
    setIsPlaying(true);
  };

  return (
    <Card className="w-full max-w-lg p-4">
      <div className="text-center mb-4">
        <h2 className="text-2xl font-bold mb-2">Portfolio Runner</h2>
        <div className="flex justify-center gap-4 mb-2">
          <div className="flex items-center gap-2">
            <Trophy className="text-yellow-500" />
            <span>Score: {score}</span>
          </div>
          <div className="flex items-center gap-2">
            <Heart className="text-red-500" />
            <span>Lives: {lives}</span>
          </div>
        </div>
      </div>

      <div 
        className="relative bg-gray-100 w-full h-80 rounded-lg overflow-hidden cursor-pointer"
        onClick={handleJump}
      >
        {/* Player */}
        <div 
          className="absolute w-8 h-8 bg-blue-500 rounded-full transition-all duration-100"
          style={{ 
            left: '50px',
            top: `${position}px`,
          }}
        />
        
        {/* Obstacles */}
        {obstacles.map(obstacle => (
          <div
            key={obstacle.id}
            className="absolute w-6 h-6 bg-red-500 rounded"
            style={{
              left: `${obstacle.x}px`,
              top: `${obstacle.y}px`,
            }}
          />
        ))}

        {/* Game Over / Start Screen */}
        {(!isPlaying || gameOver) && (
          <div className="absolute inset-0 bg-black/50 flex flex-col items-center justify-center">
            <div className="text-white text-center mb-4">
              {gameOver ? (
                <>
                  <h3 className="text-xl mb-2">Game Over!</h3>
                  <p>Final Score: {score}</p>
                </>
              ) : (
                <h3 className="text-xl mb-2">Click to Jump!</h3>
              )}
            </div>
            <Button onClick={startGame}>
              {gameOver ? 'Play Again' : 'Start Game'}
            </Button>
          </div>
        )}
      </div>
    </Card>
  );
};

export default MiniGame;
---

### 🤝 Let's Connect!

<div align="center">
  
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/ryunovii)
[![Portfolio](https://img.shields.io/badge/Portfolio-4285F4?style=for-the-badge&logo=google-chrome&logoColor=white)](https://sites.google.com/view/portofolio-rizkiardi/)
[![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/message/TSYJ5QPWJWOOM1)

</div>

### 💻 Tech Stack

<div align="center">

#### Frontend
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white)

#### Backend
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![Java](https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=java&logoColor=white)

#### Database
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-003B57?style=for-the-badge&logo=sqlite&logoColor=white)

#### Programming
![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)

</div>

### 📊 Specialization

<div align="center">
  
![Data Analysis](https://img.shields.io/badge/Data_Analysis-2C2D72?style=for-the-badge&logo=datacamp&logoColor=white)

</div>

---

<div align="center">
  <h3>🌟 Portfolio Status</h3>
  <p><i>Laravel Portfolio is currently under maintenance. Coming soon with exciting updates!</i></p>
</div>

### 📈 GitHub Stats

<div align="center">
  
![GitHub Stats](https://github-readme-stats.vercel.app/api?username=YOUR_GITHUB_USERNAME&show_icons=true&theme=radical)
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=YOUR_GITHUB_USERNAME&layout=compact&theme=radical)

</div>

---

<div align="center">
  <p>💡 <i>Always learning, always growing!</i></p>
</div>
