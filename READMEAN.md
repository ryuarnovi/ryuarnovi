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
